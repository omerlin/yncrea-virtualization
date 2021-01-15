# Chocolatey Cheatsheet

## Useful Links


* [Setup](https://docs.chocolatey.org/en-us/choco/setup/)
  
* [Commands list](https://docs.chocolatey.org/en-us/choco/commands/)

## List Installed Packages
```
choco list --local-only
```

## Install a Package
### Standard Package Installation

```
choco install PACKAGENAME -y
```

```
choco install git -y
```

### Multiple Package Installation
```
choco install packer vagrant virtualbox git poshgit chefdk visualstudiocode -y
```

### Ignore Checksums

```
choco install github --ignore-checksums
```

## Upgrade a Package
```
choco upgrade <pkg|all> [<pkg2> <pkgN>] [<options/switches>]

choco upgrade git -y

choco upgrade all -y
```

## Uninstall a Package
```
choco uninstall PACKAGENAME

choco uninstall git --all-versions -y
```

## List outdated packages
```
choco outdated
```
