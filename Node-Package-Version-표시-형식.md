# Node Package Version 표시 형식

- `version` Must match `version` exactly
- `>version` Must be greater than `version`
- `>=version` etc
- `<version`
- `<=version`
- `~version` "Approximately equivalent to version" See semver
- `^version` "Compatible with version" See semver
- `1.2.x` 1.2.0, 1.2.1, etc., but not 1.3.0
- `http://...` See 'URLs as Dependencies' below
- `*` Matches any version
- `""` (just an empty string) Same as `*`
- `version1 - version2` Same as `>=version1 <=version2`.
- `range1 || range2` Passes if either range1 or range2 are satisfied.
- `git...` See 'Git URLs as Dependencies' below
- `user/repo` See 'GitHub URLs' below
- `tag` A specific version tagged and published as `tag` See `npm dist-tag`
- `path/path/path` See Local Paths below

# 참고

- https://docs.npmjs.com/cli/v7/configuring-npm/package-json#dependencies
- https://github.com/npm/node-semver#versions
