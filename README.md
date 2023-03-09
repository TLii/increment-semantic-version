# Increment Semantic Version - with Build Numbers

Forked from [christian-drager/increment-semantic-version](https://github.com/christian-draeger/increment-semantic-version/forks).

This is a GitHub action to bump a given semantic version or build number within a semantic version, depending on a given version fragment.

## Inputs

### `current-version`

**Required** The current semantic version you want to increment. (e.g. 3.12.5)

### `version-fragment`

**Required** The versions fragment you want to increment.

Possible options are **[ major | feature | bug | alpha | beta | pre | rc | build ]**

### `include-build-number`

*Defaults to false.* Whether you want to always include a build number. If true, all output versions will include a build number that resets with every semver change. If false, build numbers are only used if version-fragment is `build`.

## Outputs

### `next-version`

The incremented version.

## Example usage

    - name: Bump release version
      id: bump_version
      uses: christian-draeger/increment-semantic-version@1.1.0
      with:
        current-version: '2.11.7-alpha.3' # also accepted: 'v2.11.7-alpha.3' | '2.11.7-alpha3'
        version-fragment: 'feature'
    - name: Do something with your bumped release version
      run: echo ${{ steps.bump_version.outputs.next-version }}
      # will print 2.12.0

## Build numbers

**Note: Build numbers are not considered version changes!**
You can use build number in two ways.

1) By setting `include-build-number` (see above) to true, the build number is appended and either reset to 0 or incremented on every run of this action.
2) If you use `build` as `version-fragment`, semantic version does not change, but build number is incremented. The intended use is when no version change is triggered (as per semantic version specification), but the change should still be noticeable in the version statement.

If semantic version changes in any way, build number is reset and the first build is 0. **If `current-version` does not include a build number, it is assumed to be the first build (ie. 0) and will be incremented to 1 for `next-version`.**

## input / output Examples

| version-fragment | current-version          |   | output w/o build number| output w/ build number |
| ---------------- | ------------------------ | - | ---------------------- | ---------------------- |
| major            | 2.11.7                   |   | 3.0.0                  | 3.0.0+build.0          |
| major            | v2.11.7                  |   | 3.0.0                  | 3.0.0+build.0          |
| major            | 2.11.7-alpha3            |   | 3.0.0                  | 3.0.0+build.0          |
| major            | 2.11.7-alpha.3           |   | 3.0.0                  | 3.0.0+build.0          |
| major            | 2.11.7-alpha.3+build.23  |   | 3.0.0                  | 3.0.0+build.0          |
| feature          | 2.11.7                   |   | 2.12.0                 | 2.12.0+build.0         |
| feature          | 2.11.7+build.5           |   | 2.12.0                 | 2.12.0+build.0         |
| feature          | 2.11.7-alpha3            |   | 2.12.0                 | 2.12.0+build.0         |
| feature          | 2.11.7-alpha.3           |   | 2.12.0                 | 2.12.0+build.0         |
| feature          | 2.11.7-alpha.3+build.5   |   | 2.12.0                 | 2.12.0+build.0         |
| bug              | 2.11.7                   |   | 2.11.8                 | 2.11.8+build.0         |
| bug              | 2.11.7+build.2           |   | 2.11.8                 | 2.11.8+build.0         |
| bug              | 2.11.7-alpha3            |   | 2.11.8                 | 2.11.8+build.0         |
| bug              | 2.11.7-alpha.3           |   | 2.11.8                 | 2.11.8+build.0         |
| bug              | 2.11.7-alpha.3+build.7   |   | 2.11.8                 | 2.11.8+build.0         |
| alpha            | 2.11.7                   |   | 2.11.7-alpha.1         | 2.11.7-alpha.1+build.0 |
| alpha            | 2.11.7+build.2           |   | 2.11.7-alpha.1         | 2.11.7-alpha.1+build.0 |
| alpha            | 2.11.7-alpha3            |   | 2.11.7-alpha.4         | 2.11.7-alpha.1+build.0 |
| alpha            | 2.11.7-alpha.3           |   | 2.11.7-alpha.4         | 2.11.7-alpha.1+build.0 |
| alpha            | 2.11.7-alpha.3+build.3   |   | 2.11.7-alpha.4         | 2.11.7-alpha.1+build.0 |
| beta             | 2.11.7                   |   | 2.11.7-beta.1          | 2.11.7-beta.1+build.0  |
| beta             | 2.11.7+build.1           |   | 2.11.7-beta.1          | 2.11.7-beta.1+build.0  |
| beta             | 2.11.7-alpha3            |   | 2.11.7-beta.1          | 2.11.7-beta.1+build.0  |
| beta             | 2.11.7-alpha.3           |   | 2.11.7-beta.1          | 2.11.7-beta.1+build.0  |
| beta             | 2.11.7-alpha.3+build.5   |   | 2.11.7-beta.1          | 2.11.7-beta.1+build.0  |
| pre              | 2.11.7                   |   | 2.11.7-pre.1           | 2.11.7-pre.1+build.0   |
| pre              | 2.11.7+build.5           |   | 2.11.7-pre.1           | 2.11.7-pre.1+build.0   |
| pre              | 2.11.7-alpha3            |   | 2.11.7-pre.1           | 2.11.7-pre.1+build.0   |
| pre              | 2.11.7-alpha.3           |   | 2.11.7-pre.1           | 2.11.7-pre.1+build.0   |
| pre              | 2.11.7-alpha.3+build.5   |   | 2.11.7-pre.1           | 2.11.7-pre.1+build.0   |
| rc               | 2.11.7                   |   | 2.11.7-rc.1            | 2.11.7-rc.1+build.0    |
| rc               | 2.11.7+build.1           |   | 2.11.7-rc.1            | 2.11.7-rc.1+build.0    |
| rc               | 2.11.7-alpha3            |   | 2.11.7-rc.1            | 2.11.7-rc.1+build.0    |
| rc               | 2.11.7-alpha.3           |   | 2.11.7-rc.1            | 2.11.7-rc.1+build.0    |
| rc               | 2.11.7-alpha.3+build.5   |   | 2.11.7-rc.1            | 2.11.7-rc.1+build.0    |
| build            | 2.11.7                   |   | 2.11.7+build.1         | 2.11.7.1+build.1       |
| build            | 2.11.7+build.2           |   | 2.11.7+build.3         | 2.11.7.1+build.3       |
| build            | 2.11.7-alpha3            |   | 2.11.7-alpha.1+build.1 | 2.11.7-alpha.1+build.1 |
| build            | 2.11.7-alpha.3           |   | 2.11.7-alpha.1+build.1 | 2.11.7-alpha.1+build.1 |
| build            | 2.11.7-alpha.3+build.5   |   | 2.11.7-alpha.1+build.6 | 2.11.7-alpha.1+build.6 |

## License

The scripts and documentation in this project are released under the [MIT License](LICENSE)
