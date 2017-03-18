# Terminal Tips

Tips, tricks, and utilities that I've found useful in the past.

## Shell tricks


## Git tricks

### Referring to commits

There are lots of useful shortcuts for referring to commits, see [the git
docs][git revisions documentation] and [this great StackOverlow answer][git
commit-ish stackoverflow] answer for an in-depth review.

|---------------------------|------------------------------------------|
|    Commit-ish/Tree-ish    |                Examples                  |
|---------------------------|------------------------------------------|
|  1. <sha1>                | dae86e1950b1277e545cee180551750029cfe735 |
|  2. <describeOutput>      | v1.7.4.2-679-g3bee7fb                    |
|  3. <refname>             | master, heads/master, refs/heads/master  |
|  4. <refname>@{<date>}    | master@{yesterday}, HEAD@{5 minutes ago} |
|  5. <refname>@{<n>}       | master@{1}                               |
|  6. @{<n>}                | @{1}                                     |
|  7. @{-<n>}               | @{-1}                                    |
|  8. <refname>@{upstream}  | master@{upstream}, @{u}                  |
|  9. <rev>^                | HEAD^, v1.5.1^0                          |
| 10. <rev>~<n>             | master~3                                 |
| 11. <rev>^{<type>}        | v0.99.8^{commit}                         |
| 12. <rev>^{}              | v0.99.8^{}                               |
| 13. <rev>^{/<text>}       | HEAD^{/fix nasty bug}                    |
| 14. :/<text>              | :/fix nasty bug                          |

_Above table cribbed from [the StackOverflow answer][git commit-ish
stackoverflow]._

Some of the most convenient ones are:

|----------------------|---------------------------------------------|
| Short                | Equivalent                                  |
|----------------------|---------------------------------------------|
| `@`, `@~`            | `HEAD`, `HEAD~`                             |
| `:/some msg`         | First parent that contains `some msg`       |
| `branch`, `tag`      | Head of `branch` or `tag`                   |
| `branch^{/some msg}` | Search `branch` for `some msg`              |


[git
revisions documentation](https://www.kernel.org/pub/software/scm/git/docs/gitrevisions.html#_specifying_revisions)
[git commit-ish stackoverflow](http://stackoverflow.com/a/23303550/2799191)
