---
title: "<div>Customize Caddy's plugins with Nix</div>"
date: 2025-03-19
categories: 
  - "general"
---

Caddy is an open-source web server written in Go. It handles TLS certificates automatically and comes with a simple configuration syntax. Users can extend its functionality through plugins1 to add features like rate limiting, caching, and Docker integration.

While _Caddy_ is available in Nixpkgs, adding extra plugins is not simple.2 The compilation process needs Internet access, which Nix denies during build to ensure reproducibility. When trying to build the following derivation using xcaddy, a tool for building Caddy with plugins, it fails with this error: `dial tcp: lookup proxy.golang.org on [::1]:53: connection refused`.

```
{ pkgs }:
pkgs.stdenv.mkDerivation {
  name = "caddy-with-xcaddy";
  nativeBuildInputs = with pkgs; [ go xcaddy cacert ];
  unpackPhase = "true";
  buildPhase =
    ''
      xcaddy build --with github.com/caddy-dns/powerdns@v1.0.1
    '';
  installPhase = ''
    mkdir -p $out/bin
    cp caddy $out/bin
  '';
}

```

Fixed-output derivations are an exception to this rule and get network access during build. They need to specify their output hash. For example, the `fetchurl` function produces a fixed-output derivation:

```
{ stdenv, fetchurl }:
stdenv.mkDerivation rec {
  pname = "hello";
  version = "2.12.1";
  src = fetchurl {
    url = "mirror://gnu/hello/hello-${version}.tar.gz";
    hash = "sha256-jZkUKv2SV28wsM18tCqNxoCZmLxdYH2Idh9RLibH2yA=";
  };
}

```

To create a fixed-output derivation, you need to set the `outputHash` attribute. The example below shows how to output _Caddy_’s source code, with some plugin enabled, as a fixed-output derivation using `xcaddy` and `go mod vendor`.

```
pkgs.stdenvNoCC.mkDerivation rec {
  pname = "caddy-src-with-xcaddy";
  version = "2.8.4";
  nativeBuildInputs = with pkgs; [ go xcaddy cacert ];
  unpackPhase = "true";
  buildPhase =
    ''
      export GOCACHE=$TMPDIR/go-cache
      export GOPATH="$TMPDIR/go"
      XCADDY_SKIP_BUILD=1 TMPDIR="$PWD" 
        xcaddy build v${version} --with github.com/caddy-dns/powerdns@v1.0.1
      (cd buildenv* && go mod vendor)
    '';
  installPhase = ''
    mv buildenv* $out
  '';

  outputHash = "sha256-F/jqR4iEsklJFycTjSaW8B/V3iTGqqGOzwYBUXxRKrc=";
  outputHashAlgo = "sha256";
  outputHashMode = "recursive";
}

```

With a fixed-output derivation, it is up to us to ensure the output is always the same:

- we ask `xcaddy` to not compile the program and keep the source code,3
- we pin the version of Caddy we want to build, and
- we pin the version of each requested plugin.

You can use this derivation to override the `src` attribute in `pkgs.caddy`:

```
pkgs.caddy.overrideAttrs (prev: {
  src = pkgs.stdenvNoCC.mkDerivation { /* ... */ };
  vendorHash = null;
  subPackages = [ "." ];
});

```

Check out the complete example in the GitHub repository. To integrate into a Flake, add `github:vincentbernat/caddy-nix` as an overlay:

```
{
  inputs = {
    nixpkgs.url = "nixpkgs";
    flake-utils.url = "github:numtide/flake-utils";
    caddy.url = "github:vincentbernat/caddy-nix";
  };
  outputs = { self, nixpkgs, flake-utils, caddy }:
    flake-utils.lib.eachDefaultSystem (system:
      let
        pkgs = import nixpkgs {
          inherit system;
          overlays = [ caddy.overlays.default ];
        };
      in
      {
        packages = {
          default = pkgs.caddy.withPlugins {
            plugins = [ "github.com/caddy-dns/powerdns@v1.0.1" ];
            hash = "sha256-F/jqR4iEsklJFycTjSaW8B/V3iTGqqGOzwYBUXxRKrc=";
          };
        };
      });
}

```

Update (2024-11)

This flake won’t work with Nixpkgs 24.05 or older because it relies on this commit to properly override the `vendorHash` attribute.

* * *

1. This article uses the term “plugins,” though Caddy documentation also refers to them as “modules” since they are implemented as Go modules. ↩︎
    
2. This is a feature request since quite some time. A proposed solution has been rejected. The one described in this article is a bit different and I have proposed it in another pull request, which was merged. ↩︎
    
3. This is not perfect: if the source code produced by `xcaddy` changes, the hash would change and the build would fail. ↩︎
