The bug report and possible fixes can be found here: [https://github.com/ionic-team/stencil/issues/1491](https://github.com/ionic-team/stencil/issues/1491)

# stencil-lerna-poc

Stencil does not work with symlinks, I suspect that the problem is with rollup, it might require extra configurations based on this issue [#447](
https://github.com/rollup/rollup/issues/447).

The issue happens when I try to use [lerna](https://github.com/lerna/lerna) to link between packages:

I used `npm init stencil` and picked the `components` starter template for the stencil part.

```
root
    /packages
        /x-alpha (exports a string)
        /x-beta (imports x-alpha successfully)
        /x-stencil (from stencil-cli fails to import x-alpha)
```

The same issue also happens when trying to manually use `npm link` or `yarn workspaces`.

# Instructions

1. run `npm install` to install lerna in the root directory.
2. run `npm run bootstrap` to let lerna bootstrap the local dependencies.
3. go to `packages/x-beta` and run `npm start`, you should see that the symlink was connected, it will print: `successfully imported alpha: alpha`.
4. go to `packages/x-stencil`, install dependencies and call `npm start`, stencil fails to resolve symlink dependencies.
stencil gives this error in the browser console:
```
ReferenceError: module is not defined
```
