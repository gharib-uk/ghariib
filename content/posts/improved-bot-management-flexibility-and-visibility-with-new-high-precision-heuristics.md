---
title: "Improved Bot Management flexibility and visibility with new high-precision heuristics"
date: 2025-03-19
---

Within the Cloudflare Application Security team, every machine learning model we use is underpinned by a rich set of static rules that serve as a ground truth and a baseline comparison for how our models are performing. These are called heuristics. Our Bot Management heuristics engine has served as an important part of eight global machine learning (ML) models, but we needed a more expressive engine to increase our accuracy. In this post, we’ll review how we solved this by moving our heuristics to the Cloudflare Ruleset Engine. Not only did this provide the platform we needed to write more nuanced rules, it made our platform simpler and safer, and provided Bot Management customers more flexibility and visibility into their bot traffic.   

### Bot detection via simple heuristics

In Cloudflare’s bot detection, we build heuristics from attributes like software library fingerprints, HTTP request characteristics, and internal threat intelligence. Heuristics serve three separate purposes for bot detection: 

1. Bot identification: If traffic matches a heuristic, we can identify the traffic as definitely automated traffic (with a bot score of 1) without the need of a machine learning model. 
    
2. Train ML models: When traffic matches our heuristics, we create labelled datasets of bot traffic to train new models. We’ll use many different sources of labelled bot traffic to train a new model, but our heuristics datasets are one of the highest confidence datasets available to us.   
    
3. Validate models: We benchmark any new model candidate’s performance against our heuristic detections (among many other checks) to make sure it meets a required level of accuracy.
    

While the existing heuristics engine has worked very well for us, as bots evolved we needed the flexibility to write increasingly complex rules. Unfortunately, such rules were not easily supported in the old engine. Customers have also been asking for more details about which specific heuristic caught a request, and for the flexibility to enforce different policies per heuristic ID.  We found that by building a new heuristics framework integrated into the Cloudflare Ruleset Engine, we could build a more flexible system to write rules and give Bot Management customers the granular explainability and control they were asking for. 

### The need for more efficient, precise rules

In our previous heuristics engine, we wrote rules in Lua as part of our openresty\-based reverse proxy. The Lua-based engine was limited to a very small number of characteristics in a rule because of the high engineering cost we observed with adding more complexity.

With Lua, we would write fairly simple logic to match on specific characteristics of a request (i.e. user agent). Creating new heuristics of an existing class was fairly straight forward. All we’d need to do is define another instance of the existing class in our database. However, if we observed malicious traffic that required more than two characteristics (as a simple example, user-agent and ASN) to identify, we’d need to create bespoke logic for detections. Because our Lua heuristics engine was bundled with the code that ran ML models and other important logic, all changes had to go through the same review and release process. If we identified malicious traffic that needed a new heuristic class, and we were also blocked by pending changes in the codebase, we’d be forced to either wait or rollback the changes. If we’re writing a new rule for an “under attack” scenario, every extra minute it takes to deploy a new rule can mean an unacceptable impact to our customer’s business. 

More critical than time to deploy is the complexity that the heuristics engine supports. The old heuristics engine only supported using specific request attributes when creating a new rule. As bots became more sophisticated, we found we had to reject an increasing number of new heuristic candidates because we weren’t able to write precise enough rules. For example, we found a Golang TLS fingerprint frequently used by bots and by a small number of corporate VPNs. We couldn’t block the bots without also stopping the legitimate VPN usage as well, because the old heuristics platform lacked the flexibility to quickly compile sufficiently nuanced rules. Luckily, we already had the perfect solution with Cloudflare Ruleset Engine. 

### Our new heuristics engine

The Ruleset Engine is familiar to anyone who has written a WAF rule, Load Balancing rule, or Transform rule, just to name a few. For Bot Management, the Wireshark-inspired syntax allows us to quickly write heuristics with much greater flexibility to vastly improve accuracy. We can write a rule in YAML that includes arbitrary sub-conditions and inherit the same framework the WAF team uses to both ensure any new rule undergoes a rigorous testing process with the ability to rapidly release new rules to stop attacks in real-time. 

Writing heuristics on the Cloudflare Ruleset Engine allows our engineers and analysts to write new rules in an easy to understand YAML syntax. This is critical to supporting a rapid response in under attack scenarios, especially as we support greater rule complexity. Here’s a simple rule using the new engine, to detect empty user-agents restricted to a specific JA4 fingerprint (right), compared to the empty user-agent detection in the old Lua based system (left): 

|   **Old**   |   **New**   |
| --- | --- |
|   `local _M = {}`  `local EmptyUserAgentHeuristic = {`     `heuristic = {},`  `}`  `EmptyUserAgentHeuristic.__index = EmptyUserAgentHeuristic`  `--- Creates and returns empty user agent heuristic`  `-- @param params table contains parameters injected into EmptyUserAgentHeuristic`  `-- @return EmptyUserAgentHeuristic table`  `function _M.new(params)`     `return setmetatable(params, EmptyUserAgentHeuristic)`  `end`  ``--- Adds heuristic to be used for inference in `detect` method``  `-- @param heuristic schema.Heuristic table`  `function EmptyUserAgentHeuristic:add(heuristic)`     `self.heuristic = heuristic`  `end`  `--- Detect runs empty user agent heuristic detection`  `-- @param ctx context of request`  `-- @return schema.Heuristic table on successful detection or nil otherwise`  `function EmptyUserAgentHeuristic:detect(ctx)`     `local ua = ctx.user_agent`     `if not ua or ua == '' then`        `return self.heuristic`     `end`  `end`  `return _M`   |   `ref: empty-user-agent`        `description: Empty or missing`  `User-Agent header`        `action: add_bot_detection`        `action_parameters:`          `active_mode: false`        `expression: http.user_agent eq`  `"" and cf.bot_management.ja4 = "t13d1516h2_8daaf6152771_b186095e22b6"`   |

The Golang heuristic that captured corporate proxy traffic as well (mentioned above) was one of the first to migrate to the new Ruleset engine. Before the migration, traffic matching on this heuristic had a false positive rate of 0.01%. While that sounds like a very small number, this means for every million bots we block, 100 real users saw a Cloudflare challenge page unnecessarily. At Cloudflare scale, even small issues can have real, negative impact.

When we analyzed the traffic caught by this heuristic rule in depth, we saw the vast majority of attack traffic came from a small number of abusive networks. After narrowing the definition of the heuristic to flag the Golang fingerprint only when it’s sourced by the abusive networks, the rule now has a false positive rate of 0.0001% (One out of 1 million).  Updating the heuristic to include the network context improved our accuracy, while still blocking millions of bots every week and giving us plenty of training data for our bot detection models. Because this heuristic is now more accurate, newer ML models make more accurate decisions on what’s a bot and what isn’t.

### New visibility and flexibility for Bot Management customers 

While the new heuristics engine provides more accurate detections for all customers and a better experience for our analysts, moving to the Cloudflare Ruleset Engine also allows us to deliver new functionality for Enterprise Bot Management customers, specifically by offering more visibility. This new visibility is via a new field for Bot Management customers called Bot Detection IDs. Every heuristic we use includes a unique Bot Detection ID. These are visible to Bot Management customers in analytics, logs, and firewall events, and they can be used in the firewall to write precise rules for individual bots. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3cYXHw8tFUjdJQGm93gFcE/0a3f6ab89a70410ebb7dd2c6f4f3a677/1.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/9d7L1r7yN9AEhO26H7SgQ/434f0f934dd4f4498a8d13e85a7660ae/2.png)

Detections also include a specific tag describing the class of heuristic. Customers see these plotted over time in their analytics.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2UlkGQ070UWsmU76IXYqDd/6bca8f28959a8fe8f3013792a17b348a/image4.png)

To illustrate how this data can help give customers visibility into why we blocked a request, here’s an example request flagged by Bot Management (with the IP address, ASN, and country changed):

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3i6k9ejsBVwlbXUrZFIFJG/4c9cddd11d9f7a8ddf10eb5ff30a027b/4.png)

Before, just seeing that our heuristics gave the request a score of 1 was not very helpful in understanding why it was flagged as a bot. Adding our Detection IDs to Firewall Events helps to paint a better picture for customers that we’ve identified this request as a bot because that traffic used an empty user-agent.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/5UQd20VnXCnIav1skHiX6i/18e0cf01106601ae7faf18be50d43365/5.png)

In addition to Analytics and Firewall Events, Bot Detection IDs are now available for Bot Management customers to use in Custom Rules, Rate Limiting Rules, Transform Rules, and Workers. 

#### Account takeover detection IDs

One way we’re focused on improving Bot Management for our customers is by surfacing more attack-specific detections. During Birthday Week, we launched Leaked Credentials Check for all customers so that security teams could help prevent account takeover (ATO) attacks by identifying accounts at risk due to leaked credentials. We’ve now added two more detections that can help Bot Management enterprise customers identify suspicious login activity via specific detection IDs that monitor login attempts and failures on the zone. These detection IDs are not currently affecting the bot score, but will begin to later in 2025. Already, they can help many customers detect more account takeover events now.

Detection ID **201326592** monitors traffic on a customer website and looks for an anomalous rise in login failures (usually associated with brute force attacks), and ID **201326593** looks for an anomalous rise in login attempts (usually associated with credential stuffing). 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4a2LGgB1bXGwFgHQEKFSI9/ff4194a6a470e466274f7b7c51e9dc18/6.png)

### Protect your applications

If you are a Bot Management customer, log in and head over to the Cloudflare dashboard and take a look in Security Analytics for bot detection IDs `201326592` and `201326593`.

These will highlight ATO attempts targeting your site. If you spot anything suspicious, or would like to be protected against future attacks, create a rule that uses these detections to keep your application safe.

Go to Source
