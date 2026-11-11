# Lilys's Wallpaper Collection
A collection of silly wallpapers I like from across the internet

(Credits go to my beautiful girlfriend fern for making this system for me <3)

## Usage

### Flakes
This repository provides a nix flake which can easily be imported in your own flake.

`flake.nix`
```nix
  inputs = {
    wallpapers = {
      url = "github:FernLeafie/wallpapers";
      inputs.nixpkgs.follows = "nixpkgs";
    };
  };
```
And use it somewhere, either globally, or with Home Manager:

`configuration.nix`
```nix
{inputs, pkgs, ...}: {
  environment = {
    etc."wallpapers".source = inputs.wallpapers.packages.${pkgs.system}.full;
  };
}
```
or 

`home.nix`
```nix
{inputs, pkgs, ...}: {
  home.file."${config.xdg.userDirs.pictures}/wallpapers".source =
    inputs.wallpapers.packages.${pkgs.system}.full;
}
```
However, as the wallpapers are stored in the same repository as the flake declaration, this will download the repo at evaluation, seemingly regardless if it's used or not.
### Local Package
To avoid this, on for example a multi host/user flake. We can add a package declaration of our own to our flake, which only gets built if called.
So together with a build file a little like this: [/wallpapers/fern.nix](https://github.com/FernLeafie/snowflake/blob/main/modules/home/wallpapers/fern.nix)

We can invoke the package using a relative path a little like this.

`home.nix`
```nix
{inputs, pkgs, ...}: {
  home.file."${config.xdg.userDirs.pictures}/wallpapers".source = "${
    (pkgs.callPackage ./wallpapers/fern.nix { })
  }";
}
```

## Credits
This work is largely based on the works of [42willow/wallpapers](https://github.com/42willow/wallpapers) and [NotAShelf/wallpkgs](https://github.com/NotAShelf/wallpkgs), although is significantly less clever or smart in execution. So a big thanks to 42willow and NotAShelf :3

### Tooling used
- [lutgen](https://github.com/ozwaldorf/lutgen-rs)
- [oxipng](https://github.com/oxipng/oxipng)
