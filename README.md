# action-setup

![version](.github/ojobs/version.svg) [![Tests](https://github.com/OpenAF/action-setup/actions/workflows/tests.yml/badge.svg)](https://github.com/OpenAF/action-setup/actions/workflows/tests.yml)

GitHub action to setup [OpenAF](https://docs.openaf.io).

## Usage

```yaml
  - name: Setup OpenAF
    uses: openaf/action-setup@v1.2

  - name: Using OpenAF
    run : |
      echo Recursive list of files order by size:
      echo
      oafp data="." in=ls lsrecursive=true\
           sql="select filepath, permissions, size where isFile=TRUE order by size desc"\
           out=ctable
```

### Available commands

After setup you will be able to use:

| Command | Description |
|---------|-------------|
| oaf     | The main OpenAF runtime |
| oafp    | The OpenAF data processor |
| ojob    | The OpenAF's oJob orchestrator (although using [openaf/ojob-action](https://github.com/OpenAF/ojob-action) instead is recommended) |
| opack   | The OpenAF's package manager |
| oaf-sb  | The main OpenAF runtime unix she-bang executor |
| ojob-sb | The OpenAF's oJob unix she-bang executor |
| oafp-sb | The OpenAF data processor she-bang executor |

### Caching

To cache the OpenAF installation you can use [GitHub Actions caching](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows) (saving, at least, around ~4/5 seconds of action execution) but you will have to be carefull with the management of the cache. Example:

```yaml
  - name: Cache OpenAF
    uses: actions/cache@v4
    with:
      # you need to manage the cache in the best way specifically for your case
      key : oaf
      path: /tmp/oaf

  - name: Setup OpenAF
    uses: openaf/action-setup@v1.2
```

### Using a different distribution

By default the 'stable' OpenAF distribution is used but you can specify a different one using the _dist_ input:

```yaml
  - name: Setup OpenAF
    uses: openaf/action-setup@v1.2
    with:
      dist: nightly

  - name: Check OpenAF distribution
    run : |
      echo "The current OpenAF distribution is '$(oaf -c 'print(getDistribution())')'"
```

### Using a specific version

By default the latest version of the defined (or stable) distribution will be retrived. To specify it you can use the _version_ input:

```yaml
  - name: Setup OpenAF
    uses: openaf/action-setup@v1.2
    with:
      version: 20241117

  - name: Check OpenAF version
    run : |
      echo "The current OpenAF version is '$(oaf -c 'print(getVersion())')'"
```

### Installing specific oPacks

You can also list oPacks to be installed as part of the setup process:

```yaml
  - name: Setup OpenAF
    uses: openaf/action-setup@v1.2
    with:
      opacks: oJob-common,GIST

  - name: Check list of installed oPacks
    run : |
      opack list
```

> Works together with https://github.com/OpenAF/ojob-action
