---
title: "Nginx Restic Backend"
date: Fri, 04 Oct 2019 17:50:15 GMT
draft: false
type: posts
---
# Nginx Restic Backend

<br/>

<br/>
I’ve started using an excellent piece of software called [Restic](https://restic.net/) for backing up my various hosts. Restic has multiple backend types that you can send your backups to. One of the backends it supports is a [REST API](https://restic.readthedocs.io/en/latest/100_references.html#rest-backend) for which there is an implementation named [Rest Server](https://github.com/restic/rest-server) written in [Go](https://golang.org/).

I thought to myself, if it’s just a simple REST API, why do I need to learn/install/manage a new piece of software? I already use [Nginx](https://nginx.org/) all over the place. Can I just use Nginx for this? The answer was yes.

I have configured two nginx vhosts, and run them on different ports. One of the vhosts is to be accessed by hosts which are backing themselves up. It doesn’t allow them to delete objects (other than lock files), or overwrite them either. Meaning it is an “append-only” backup solution. The other vhost allows deletion, and exists for administrative tasks like pruning old backups.

For demo purposes, I’ve stripped a few things out of this config, e.g TLS. You will need to modify the config for your own use cases. Here is the the append-only config:

```
server {

    listen 0.0.0.0:80;

    # In my testing, all objects are much smaller than 1GB in size, even if the file is larger. This may be tunable.
    client_max_body_size 1000M;

    # We support Restic REST API v2, only
    default_type "application/vnd.x.restic.rest.v2";

    # Authentication
    auth_basic           "Restic Append-Only Backups";
    auth_basic_user_file /opt/backups/auth/.htpasswd;
    root                 /opt/backups/repo/$remote_user;

    # Routing
    error_page 470 = @list_objects;
    error_page 471 = @read_object;
    error_page 472 = @write_object;
    error_page 473 = @delete_object;
    error_page 474 = @put_proxy;

    # Things that can be listed
    location ~ "^/(data|keys|locks|snapshots|index)/$" {
        if ($request_method = 'GET') { return 470; } # @list_objects
        return 403;
    }

    # Reading the config file and keys
    location ~ "^/(config|keys/[a-f0-9]{64})$" {
        if ($request_method = 'HEAD') { return 471; } # @read_object
        if ($request_method = 'GET')  { return 471; } # @read_object
        return 403;
    }

    # Managing lock files
    location ~ "^/locks/[a-f0-9]{64}$" {
        if ($request_method = 'HEAD')   { return 471; } # @read_object
        if ($request_method = 'GET')    { return 471; } # @read_object
        if ($request_method = 'DELETE') { return 473; } # @delete_object
        if ($request_method = 'PUT')    { return 472; } # @write_object
        if ($request_method = 'POST')   { return 474; } # @put_proxy
        return 403;
    }

    # Reading and Writing data, index and snapshots
    location ~ "^/(data|index|snapshots)/[a-f0-9]{64}$" {
        if ($request_method = 'HEAD') { return 471; } # @read_object
        if ($request_method = 'GET')  { return 471; } # @read_object
        if ($request_method = 'PUT')  { return 472; } # @write_object
        if ($request_method = 'POST') { return 474; } # @put_proxy
        return 403;
    }

    # Block everything else
    location ~ "^" {
        return 403;
    }

    # Reading objects
    location @read_object {
    }

    # Writing objects
    location @write_object {

        # Prevent overwriting files
        if (-f $request_filename) {
            return 403 'No overwriting files';
        }

        dav_methods PUT;
        create_full_put_path on;
        dav_access user:rw;
    }

    # Deleting objects
    location @delete_object {
        dav_methods DELETE;
        header_filter_by_lua_block {
            if ngx.status == ngx.HTTP_NO_CONTENT then
                ngx.status = ngx.HTTP_OK
            end
        }
    }

    # Listing objects
    location @list_objects {
        autoindex            on;
        autoindex_exact_size on;
        autoindex_format     json;

        body_filter_by_lua_block {
            chunk = ngx.arg[1];
            if string.match(chunk, '^<') then
                chunk = '[]'
                ngx.arg[2] = true
            else
                chunk = ngx.arg[1];
                chunk = ngx.re.gsub(chunk, '\\s+', '')
                chunk = ngx.re.gsub(chunk, '"(?!name|size)[^"]+":"[^"]+"', '')
                chunk = ngx.re.gsub(chunk, ',{2,}', ',')
                chunk = ngx.re.gsub(chunk, '{,', '{')
                chunk = ngx.re.gsub(chunk, ',}', '}')
            end
            ngx.arg[1] = chunk
        }
        header_filter_by_lua_block {
            ngx.header["content-type"] = "application/vnd.x.restic.rest.v2";
            if ngx.status == ngx.HTTP_NOT_FOUND then
                ngx.status = ngx.HTTP_OK
                ngx.header["content-length"] = 2
            end
        }
    }

    set_real_ip_from 127.0.0.1;
    location @put_proxy {
        proxy_pass       http://127.0.0.1:80;
        proxy_method     PUT;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        header_filter_by_lua_block {
            if ngx.status == ngx.HTTP_CREATED then
                ngx.status = ngx.HTTP_OK
            end
        }
    }
}
```

The config is laid out such that there are a handful of regex based location blocks at the top, which return error codes based on HTTP method, which are mapped to other location blocks below, which actually perform the requested action.

I used $remote\_user in the “root” so that I could give each host it’s own set of credentials to use for basic auth, which would isolate each host into their own backup directory.

I had to use a few tricks to make Nginx compatible with the Restic REST API. First of all, I used the DAV module, to allow Nginx to write files. I configured it with “create\_full\_put\_path on” so that it would recursively create parent directories. The DAV module expects PUT requests, whilst the restic client uses POST requests. There is no way of forcing the DAV module to work with POST requests, so I set up the “@put\_proxy” location block, which proxies requests to localhost whilst modifying the method from POST to PUT. Not ideal, but it works. Unfortunately the DAV module returns a 204 response code, but the restic client fails unless it gets a 200, so I used the LUA Nginx module to modify the response code. I had to do this for deleting objects too.

The other main difficulty was that there are certain end-points that the restic client expects to be able to get directory listings from, in a specific format. To achieve this, I used the autoindex functionality built into Nginx (in the @list\_objects location block), and set it to return JSON instead of the default HTML. Luckily, the JSON format supplied by Nginx’s autoindex, is close to the one expected by Restic. It returns an array containing objects with “name” and “size” fields. Unfortunately, it returns a number of other fields, which restic isn’t expecting, and the restic client bombs out because of them. To deal with this, I used the Nginx LUA module to modify the response body to remove the unexpected fields, using regexes. Hacky, but works. I also had to configure it to return a 200 response with an empty array, instead of a 404 if the directory doesn’t exist.

The admin vhost is very similar with a few small differences:

1.  I removed $remote\_user from the “root”, meaning the admin user can access all backups at slightly different paths.
2.  I used a separate htpasswd file containing only the admin user credentials.
3.  I prefixed each location regex block to allow for parent paths containing my hostnames. E.g “^/locks/\[a-f0-9\]{64}$” becomes “^/\[a-z0-9\]+/locks/\[a-f0-9\]{64}$”
4.  I added the ability to delete and write to more of the location blocks, e.g to allow writing to the config/keys and deleting from index/data/snapshots.
5.  I added a new location block at the start to allow “restic init”. It doesn’t actually need to do anything, but it needs to return a 200:

```
location ~ "^/[a-z0-9]+/$" {
    if ($request_method = 'POST') { return 200 'Fake initialised'; }
    return 403;
}
```

I’m not going to quote the entire admin config above. You should be able to figure it out yourself from my description if you’ve understood the config.

I mentioned the Nginx DAV and LUA modules above. Don’t worry, on Debian at least, DAV is built in, and the LUA module can be used simply by doing an “apt install nginx libnginx-mod-http-lua”

### Performance?

I don’t have anything interesting to give you. I did some basic performance tests and didn’t find any noticable differences between the two solutions. For your use case, you may find one better than the other. Nginx is certainly a lot more tunable/configurable/customisable than Rest Server, so you may be able to do some more interesting things there. Especially when it comes to things like rate limiting or proxying to multiple backends etc.

### Security?

If one of my hosts is compromised, that sucks. The attacker will be able to access and delete the contents of my server, but because the backups are append-only, at least they wont be able to delete them too.

If my backup host is compromised, that sucks. The attacker will be able to delete my backups. But they wont be able to read them because they’re encrypted client side. And my data should still exist on the hosts that are being backed up.

Administrative commands like pruning backups are run from a separate host. Hopefully that host wont become compromised as it currently has access to read, decrypt and delete all of my backups. I’m not going to say much about that host ;)

### Simplifying the config

With a few small backwards compatible changes to the restic client and API, I could remove the LUA dependency, and the need to proxy write requests. I might have a look into doing this and seeing if my changes can be upstreamed, when I find some time.

### Hosting

I’m using a Lithuanian VPS Host named [Time4VPS](https://www.time4vps.com/?affid=4501) (affiliate link) to host these backups. They have a set of very cheap “Storage VPS” plans specifically for things like backups. For example, they do a €5.99/month plan which gives you 1TB of disk and 8TB of bandwidth. They go all the way up to 64TB of disk per server. You can select from a variety of distros (including Debian and Centos), and are given full root access.

[![](https://www.grepular.com/images/amazon/mastering_nginx.jpg)](https://www.grepular.com/redir?key=amazon_mastering_nginx "Mastering Nginx")

[![](https://www.grepular.com/images/amazon/lua_quick_start.jpg)](https://www.grepular.com/redir?key=amazon_lua_quick_start "LUA Quick Start")

#### [Source](https://www.grepular.com/Nginx_Restic_Backend)

<br/>
---
