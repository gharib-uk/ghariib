---
title: "<div>TanStack-Start & KeycloakJS client library</div>"
date: 2025-01-22
---

The problem with my other TanStack start post was: I did not use the keycloakJS official client library.

Plus I wanted a solution that also works with TanStack-Router loading, pre-fetching and router-based context.

Here is the example where it all comes together:

https://github.com/progwise/tanstack-start-keycloak

It turned out that the biggest issue is to invalidate the router-context correctly. It was not enough to call

`router.invalidate()`

on

`keycloak.onAuthSuccess`\`

But I needed to re-calculate the context using some beforeLoad:

\`  

```
export const Route = createFileRoute("/")({
  component: Home,
  beforeLoad: ({ context }) => {
    console.log("index beforeload");
    return { ...context, auth: { ...context.auth } };
  },
  loader: async ({ context }) => {
    console.log("index auth loader", context.auth);
  },
});
```

  
\`

If you just leave out this `beforeLoad` login never shows up on the page.

The heart of my solution is wiring up keycloak on createRouter in router.tsx.

\`  

```
export function createRouter() {
  const queryClient = new QueryClient();

  const keycloak = getKeycloak();
  const auth = {
    isLoggendIn: !!keycloak.authenticated,
    login: () => {
      keycloak.login();
    },
    logout: () => {
      keycloak.logout();
    },
    user: {
      username: keycloak.tokenParsed?.preferred_username,
      name: keycloak.tokenParsed?.name,
      email: keycloak.tokenParsed?.email,
    },
  };
  const router = createTanStackRouter({
    routeTree,
    context: { queryClient, auth },
    defaultPreload: "intent",
    defaultErrorComponent: (error) => (
      <div>Ups, something went wrong! {JSON.stringify(error, null, 2)}</div>
    ),
    defaultNotFoundComponent: () => <div>Not found!</div>,
    defaultPendingComponent: () => <div>Loading...</div>,
  });

  keycloak.onAuthSuccess = async () => {
    auth.isLoggendIn = true;
    auth.user = {
      username: keycloak.tokenParsed?.preferred_username,
      name: keycloak.tokenParsed?.name,
      email: keycloak.tokenParsed?.email,
    };
    await router.invalidate({});
  };
  return routerWithQueryClient(router, queryClient);
}
```

  
\`

My trick is:

1. Create some keycloakJS instance
2. Create an auth context object with references to the router
3. Create the router using createTanStackRouter
4. invalidate the router \`keycloak.onAuthSuccess'

But it was clear that is not enough to amend the auth-object of the context. The context itself has to keep track if the auth-object changed. I could not solve it but my workaround was the beforeLoad instruction in the component that handles the login.  

```
export const Route = createFileRoute("/")({
  component: Home,
  beforeLoad: ({ context }) => {
    console.log("index beforeload");
    return { ...context, auth: { ...context.auth } };
  },
  loader: async ({ context }) => {
    console.log("index auth loader", context.auth);
  },
});
```

Would be glad to see if someone comes up with a better solution on that.

The keycloak Object Creation needs special attention, because it expects a browser and on TanStack-Start you never know ;)  

```
import Keycloak from "keycloak-js";

const getKeycloak = () => {
  const keycloak = new Keycloak({
    url: "http://localhost:8280",
    realm: "master",
    clientId: "tanstack-start-keycloak",
  });

  if (typeof window !== "undefined") {
    keycloak.init({
      onLoad: "check-sso",
    });
  }
  return keycloak;
};

export { getKeycloak };

```

Go to Source
