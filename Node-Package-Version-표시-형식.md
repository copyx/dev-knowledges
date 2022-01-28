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

## [Caret Ranges ^1.2.3 ^0.2.5 ^0.0.4](https://github.com/npm/node-semver#caret-ranges-123-025-004)

[major, minor, patch] tuple에서 0이 아닌 가장 왼쪽 요소를 수정하지 않는 변경을 허용한다. 다르게 말하면, 1.0.0 이상 버전에서 패치와 마이너 업데이트를 허용하고, 0.X >=0.1.0 버전에서 패치 업데이트를 허용하고, 0.0.X 버전에는 업데이트를 허용하지 않는다.

많은 작성자들은 x가 주요 breaking-change 지표인 것처럼 0.x 버전을 취급한다.

Caret 범위는 작성자가 0.2.4와 0.3.0 사이에 breaking changes를 만들 수도 있을 때 이상적이며, 일반적인 관행이다. 하지만, 이는 0.2.4와 0.2.5 사이에 breaking changes가 없을 것이란 것을 가정한다. 일반적으로 관찰되는 관행에 따르면, 이는 부가적인 것으로 추측되는 변경들을 허용한다.

^1.2.3 := >=1.2.3 <2.0.0-0
^0.2.3 := >=0.2.3 <0.3.0-0
^0.0.3 := >=0.0.3 <0.0.4-0
^1.2.3-beta.2 := >=1.2.3-beta.2 <2.0.0-0 Note that prereleases in the 1.2.3 version will be allowed, if they are greater than or equal to beta.2. So, 1.2.3-beta.4 would be allowed, but 1.2.4-beta.2 would not, because it is a prerelease of a different [major, minor, patch] tuple.
^0.0.3-beta := >=0.0.3-beta <0.0.4-0 Note that prereleases in the 0.0.3 version only will be allowed, if they are greater than or equal to beta. So, 0.0.3-pr.2 would be allowed.
When parsing caret ranges, a missing patch value desugars to the number 0, but will allow flexibility within that value, even if the major and minor versions are both 0.

^1.2.x := >=1.2.0 <2.0.0-0
^0.0.x := >=0.0.0 <0.1.0-0
^0.0 := >=0.0.0 <0.1.0-0
A missing minor and patch values will desugar to zero, but also allow flexibility within those values, even if the major version is zero.

^1.x := >=1.0.0 <2.0.0-0
^0.x := >=0.0.0 <1.0.0-0

### Breaking-change?

> The first point is to decide what "breaking change" means in English\*. In some places it would be merely something which stops the code from compiling / running.
>
> From your list so far, I suppose you mean a change that will require other people to make a corresponding change. In that case, since each module of your product should have a well defined interface (be it to other modules, a public REST interface, the system's filesystem, a gui, a webapp, etc), then a breaking change is anything that removes something from one of those interfaces (or adds a new requirement for their use) - in effect, if you cannot take the previous version of the product and swap just the module with the change in, then it is a breaking one.
>
> So, yes, database changes will typically be breaking changes (unless there is code to auto-upgrade and possibly auto-downgrade as required).
>
> The main point is what is not a breaking change - changes within a module or to undocumented interfaces (i.e. ones that should not be used) are not breaking changes. If such things do break your product then there is a failure of encapsulation.
>
> \*or your (human) language of choice.
>
> From [What is a breaking change in software? - Stack Overflow](https://stackoverflow.com/questions/21703216/what-is-a-breaking-change-in-software)

한 줄 요약: 연관된 다른 곳의 변경을 유발하는 변경

# 참고

- https://docs.npmjs.com/cli/v7/configuring-npm/package-json#dependencies
- https://github.com/npm/node-semver#versions
