# Electron - Development

**Pages:** 22

---

## Using clang-tidy on C++ Code

**URL:** https://www.electronjs.org/docs/latest/development/clang-tidy

**Contents:**
- Using clang-tidy on C++ Code

clang-tidy is a tool to automatically check C/C++/Objective-C code for style violations, programming errors, and best practices.

Electron's clang-tidy integration is provided as a linter script which can be run with npm run lint:clang-tidy. While clang-tidy checks your on-disk files, you need to have built Electron so that it knows which compiler flags were used. There is one required option for the script --output-dir, which tells the script which build directory to pull the compilation information from. A typical usage would be: npm run lint:clang-tidy --out-dir ../out/Testing

With no filenames provided, all C/C++/Objective-C files will be checked. You can provide a list of files to be checked by passing the filenames after the options: npm run lint:clang-tidy --out-dir ../out/Testing shell/browser/api/electron_api_app.cc

While clang-tidy has a long list of possible checks, in Electron only a few are enabled by default. At the moment Electron doesn't have a .clang-tidy config, so clang-tidy will find the one from Chromium at src/.clang-tidy and use the checks which Chromium has enabled. You can change which checks are run by using the --checks= option. This is passed straight through to clang-tidy, so see its documentation for full details. Wildcards can be used, and checks can be disabled by prefixing a -. By default any checks listed are added to those in .clang-tidy, so if you'd like to limit the checks to specific ones you should first exclude all checks then add back what you want, like --checks=-*,performance*.

Running clang-tidy is rather slow - internally it compiles each file and then runs the checks so it will always be some factor slower than compilation. While you can use parallel runs to speed it up using the --jobs|-j option, clang-tidy also uses a lot of memory during its checks, so it can easily run into out-of-memory errors. As such the default number of jobs is one.

---

## Pull Requests

**URL:** https://www.electronjs.org/docs/latest/development/pull-requests

**Contents:**
- Pull Requests
- Setting up your local environment​
  - Step 1: Fork​
  - Step 2: Build​
  - Step 3: Branch​
- Making Changes​
  - Step 4: Code​
  - Step 5: Commit​
    - Commit signing​
    - Commit message guidelines​

Fork the project on GitHub and clone your fork locally.

Build steps and dependencies differ slightly depending on your operating system. See these detailed guides on building Electron locally:

Once you've built the project locally, you're ready to start making changes!

To keep your development environment organized, create local branches to hold your work. These should be branched directly off of the main branch.

Most pull requests opened against the electron/electron repository include changes to either the C/C++ code in the shell/ folder, the JavaScript code in the lib/ folder, the documentation in docs/api/ or tests in the spec/ folder.

Please be sure to run npm run lint from time to time on any code changes to ensure that they follow the project's code style.

See coding style for more information about best practice when modifying code in different parts of the project.

It is recommended to keep your changes grouped logically within individual commits. Many contributors find it easier to review changes that are split across multiple commits. There is no limit to the number of commits in a pull request.

Note that multiple commits get squashed when they are landed.

The electron/electron repo enforces commit signatures for all incoming PRs. To sign your commits, see GitHub's documentation on Telling Git about your signing key.

A good commit message should describe what changed and why. The Electron project uses semantic commit messages to streamline the release process.

Before a pull request can be merged, it must have a pull request title with a semantic prefix.

Examples of commit messages with semantic prefixes:

Other things to keep in mind when writing a commit message:

A commit that has the text BREAKING CHANGE: at the beginning of its optional body or footer section introduces a breaking API change (correlating with Major in semantic versioning). A breaking change can be part of commits of any type. e.g., a fix:, feat: & chore: types would all be valid, in addition to any other type.

See conventionalcommits.org for more details.

Once you have committed your changes, it is a good idea to use git rebase (not git merge) to synchronize your work with the main repository.

This ensures that your working branch has the latest changes from electron/electron main.

Bug fixes and features should always come with tests. A testing guide has been provided to make the process easier. Looking at other tests to see how they should be structured can also help.

Before submitting your changes in a pull request, always run the full test suite. To run the tests:

Make sure the linter does not report any issues and that all tests pass. Please do not submit patches that fail either check.

If you are updating tests and want to run a single spec to check it:

The above would only run spec modules matching menu, which is useful for anyone who's working on tests that would otherwise be at the very end of the testing cycle.

Once your commits are ready to go -- with passing tests and linting -- begin the process of opening a pull request by pushing your working branch to your fork on GitHub.

From within GitHub, opening a new pull request will present you with a template that should be filled out. It can be found here.

If you do not adequately complete this template, your PR may be delayed in being merged as maintainers seek more information or clarify ambiguities.

You will probably get feedback or requests for changes to your pull request. This is a big part of the submission process so don't be discouraged! Some contributors may sign off on the pull request right away. Others may have detailed comments or feedback. This is a necessary part of the process in order to evaluate whether the changes are correct and necessary.

To make changes to an existing pull request, make the changes to your local branch, add a new commit with those changes, and push those to your fork. GitHub will automatically update the pull request.

There are a number of more advanced mechanisms for managing commits using git rebase that can be used, but are beyond the scope of this guide.

Feel free to post a comment in the pull request to ping reviewers if you are awaiting an answer on something. If you encounter words or acronyms that seem unfamiliar, refer to this glossary.

All pull requests require approval from a Code Owner of the area you modified in order to land. Whenever a maintainer reviews a pull request they may request changes. These may be small, such as fixing a typo, or may involve substantive changes. Such requests are intended to be helpful, but at times may come across as abrupt or unhelpful, especially if they do not include concrete suggestions on how to change them.

Try not to be discouraged. If you feel that a review is unfair, say so or seek the input of another project contributor. Often such comments are the result of a reviewer having taken insufficient time to review and are not ill-intended. Such difficulties can often be resolved with a bit of patience. That said, reviewers should be expected to provide helpful feedback.

In order to land, a pull request needs to be reviewed and approved by at least one Electron Code Owner and pass CI. After that, if there are no objections from other contributors, the pull request can be merged.

Congratulations and thanks for your contribution!

Every pull request is tested on the Continuous Integration (CI) system to confirm that it works on Electron's supported platforms.

Ideally, the pull request will pass ("be green") on all of CI's platforms. This means that all tests pass and there are no linting errors. However, it is not uncommon for the CI infrastructure itself to fail on specific platforms or for so-called "flaky" tests to fail ("be red"). Each CI failure must be manually inspected to determine the cause.

CI starts automatically when you open a pull request, but only core maintainers can restart a CI run. If you believe CI is giving a false negative, ask a maintainer to restart the tests.

**Examples:**

Example 1 (python):
```python
$ git clone git@github.com:username/electron.git$ cd electron$ git remote add upstream https://github.com/electron/electron.git$ git fetch upstream
```

Example 2 (unknown):
```unknown
$ git checkout -b my-branch -t upstream/main
```

Example 3 (unknown):
```unknown
$ git add my/changed/files$ git commit
```

Example 4 (unknown):
```unknown
$ git fetch upstream$ git rebase upstream/main
```

---

## Coding Style

**URL:** https://www.electronjs.org/docs/latest/development/coding-style

**Contents:**
- Coding Style
- General Code​
- C++ and Python​
- Documentation​
- JavaScript​
- Naming Things​

These are the style guidelines for coding in Electron.

You can run npm run lint to show any style issues detected by cpplint and eslint.

For C++ and Python, we follow Chromium's Coding Style. There is also a script script/cpplint.py to check whether all files conform.

The Python version we are using now is Python 3.9.

The C++ code uses a lot of Chromium's abstractions and types, so it's recommended to get acquainted with them. A good place to start is Chromium's Important Abstractions and Data Structures document. The document mentions some special types, scoped types (that automatically release their memory when going out of scope), logging mechanisms etc.

You can run npm run lint:docs to ensure that your documentation changes are formatted correctly.

Electron APIs uses the same capitalization scheme as Node.js:

When creating a new API, it is preferred to use getters and setters instead of jQuery's one-function style. For example, .getText() and .setText(text) are preferred to .text([text]). There is a discussion on this.

---

## Electron API History Migration Guide

**URL:** https://www.electronjs.org/docs/latest/development/api-history-migration-guide

**Contents:**
- Electron API History Migration Guide
- API history information​
  - Breaking Changes​
  - Additions​
- Example​

This document demonstrates how to add API History blocks to existing APIs.

Here are some resources you can use to find information on the history of an API:

The associated API is already removed, we will ignore that for the purpose of this example.

If we search through breaking-changes.md we can find a function that was deprecated in Electron 25.0.

We can then use git blame to find the Pull Request associated with that entry:

Verify that the Pull Request is correct and make a corresponding entry in the API History:

Refer to the API History section of style-guide.md for information on how to create API History blocks.

You can keep looking through breaking-changes.md to find other breaking changes and add those in.

You can also use git log -L :<funcname>:<file>:

Verify that the Pull Request is correct and make a corresponding entry in the API History:

We will then look for when the API was originally added:

Alternatively, you can use git blame:

Verify that the Pull Request is correct and make a corresponding entry in the API History:

**Examples:**

Example 1 (markdown):
```markdown
<!-- docs/breaking-changes.md -->### Deprecated: `BrowserWindow.getTrafficLightPosition()``BrowserWindow.getTrafficLightPosition()` has been deprecated, the`BrowserWindow.getWindowButtonPosition()` API should be used insteadwhich returns `null` instead of `{ x: 0, y: 0 }` when there is no customposition.<!-- docs/api/browser-window.md  -->#### `win.getTrafficLightPosition()` _macOS_ _Deprecated_Returns `Point` - The custom position for the traffic light buttons inframeless window, `{ x: 0, y: 0 }` will be returned when there is no customposition.
```

Example 2 (bash):
```bash
$ grep -n "BrowserWindow.getTrafficLightPosition" docs/breaking-changes.md 523:### Deprecated: `BrowserWindow.getTrafficLightPosition()`525:`BrowserWindow.getTrafficLightPosition()` has been deprecated, the$ git blame -L523,524 -- docs/breaking-changes.md1e206deec3e (Keeley Hammond 2023-04-06 21:23:29 -0700 523) ### Deprecated: `BrowserWindow.getTrafficLightPosition()`1e206deec3e (Keeley Hammond 2023-04-06 21:23:29 -0700 524)$ git log -1 1e206deec3ecommit 1e206deec3ef142460c780307752a84782f9baed (tag: v26.0.0-nightly.20230407)Author: Keeley Hammond <vertedinde@electronjs.org>Date:   Thu Apr 6 21:23:29 2023 -0700    docs: update E24/E25 breaking changes (#37878) <-- This is the associated Pull Request
```

Example 3 (markdown):
```markdown
#### `win.getTrafficLightPosition()` _macOS_ _Deprecated_<!--```YAML historydeprecated:  - pr-url: https://github.com/electron/electron/pull/37878    breaking-changes-header: deprecated-browserwindowgettrafficlightposition```-->Returns `Point` - The custom position for the traffic light buttons inframeless window, `{ x: 0, y: 0 }` will be returned when there is no customposition.
```

Example 4 (bash):
```bash
$ git log --reverse -L :GetTrafficLightPosition:shell/browser/native_window_mac.mmcommit e01b1831d96d5d68f54af879b00c617358df5372Author: Cheng Zhao <zcbenz@gmail.com>Date:   Wed Dec 16 14:30:39 2020 +0900    feat: make trafficLightPosition work for customButtonOnHover (#26789)
```

---

## Debugging on macOS

**URL:** https://www.electronjs.org/docs/latest/development/debugging-on-macos

**Contents:**
- Debugging on macOS
- Requirements​
- Attaching to and Debugging Electron​
  - Setting Breakpoints​
  - Further Reading​

If you experience crashes or issues in Electron that you believe are not caused by your JavaScript application, but instead by Electron itself, debugging can be a little bit tricky especially for developers not used to native/C++ debugging. However, using lldb and the Electron source code, you can enable step-through debugging with breakpoints inside Electron's source code. You can also use XCode for debugging if you prefer a graphical interface.

A testing build of Electron: The easiest way is usually to build it from source, which you can do by following the instructions in the build instructions. While you can attach to and debug Electron as you can download it directly, you will find that it is heavily optimized, making debugging substantially more difficult. In this case the debugger will not be able to show you the content of all variables and the execution path can seem strange because of inlining, tail calls, and other compiler optimizations.

Xcode: In addition to Xcode, you should also install the Xcode command line tools. They include LLDB, the default debugger in Xcode on macOS. It supports debugging C, Objective-C and C++ on the desktop and iOS devices and simulator.

.lldbinit: Create or edit ~/.lldbinit to allow Chromium code to be properly source-mapped.

To start a debugging session, open up Terminal and start lldb, passing a non-release build of Electron as a parameter.

LLDB is a powerful tool and supports multiple strategies for code inspection. For this basic introduction, let's assume that you're calling a command from JavaScript that isn't behaving correctly - so you'd like to break on that command's C++ counterpart inside the Electron source.

Relevant code files can be found in ./shell/.

Let's assume that you want to debug app.setName(), which is defined in browser.cc as Browser::SetName(). Set the breakpoint using the breakpoint command, specifying file and line to break on:

Then, start Electron:

The app will immediately be paused, since Electron sets the app's name on launch:

To show the arguments and local variables for the current frame, run frame variable (or fr v), which will show you that the app is currently setting the name to "Electron".

To do a source level single step in the currently selected thread, execute step (or s). This would take you into name_override_.empty(). To proceed and do a step over, run next (or n).

NOTE: If you don't see source code when you think you should, you may not have added the ~/.lldbinit file above.

To finish debugging at this point, run process continue. You can also continue until a certain line is hit in this thread (thread until 100). This command will run the thread in the current frame till it reaches line 100 in this frame or stops if it leaves the current frame.

Now, if you open up Electron's developer tools and call setName, you will once again hit the breakpoint.

LLDB is a powerful tool with a great documentation. To learn more about it, consider Apple's debugging documentation, for instance the LLDB Command Structure Reference or the introduction to Using LLDB as a Standalone Debugger.

You can also check out LLDB's fantastic manual and tutorial, which will explain more complex debugging scenarios.

**Examples:**

Example 1 (markdown):
```markdown
# e.g: ['~/electron/src/tools/lldb']script sys.path[:0] = ['<...path/to/electron/src/tools/lldb>']script import lldbinit
```

Example 2 (unknown):
```unknown
$ lldb ./out/Testing/Electron.app(lldb) target create "./out/Testing/Electron.app"Current executable set to './out/Testing/Electron.app' (x86_64).
```

Example 3 (typescript):
```typescript
(lldb) breakpoint set --file browser.cc --line 117Breakpoint 1: where = Electron Framework`atom::Browser::SetName(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> > const&) + 20 at browser.cc:118, address = 0x000000000015fdb4
```

Example 4 (cpp):
```cpp
(lldb) runProcess 25244 launched: '/Users/fr/Code/electron/out/Testing/Electron.app/Contents/MacOS/Electron' (x86_64)Process 25244 stopped* thread #1: tid = 0x839a4c, 0x0000000100162db4 Electron Framework`atom::Browser::SetName(this=0x0000000108b14f20, name="Electron") + 20 at browser.cc:118, queue = 'com.apple.main-thread', stop reason = breakpoint 1.1    frame #0: 0x0000000100162db4 Electron Framework`atom::Browser::SetName(this=0x0000000108b14f20, name="Electron") + 20 at browser.cc:118   115 	}   116   117 	void Browser::SetName(const std::string& name) {-> 118 	  name_override_ = name;   119 	}   120   121 	int Browser::GetBadgeCount() {(lldb)
```

---

## Patches in Electron

**URL:** https://www.electronjs.org/docs/latest/development/patches

**Contents:**
- Patches in Electron
- Patch justification​
- Patch system​
  - Usage​
    - Adding a new patch​
    - Editing an existing patch​
    - Removing a patch​
    - Resolving conflicts​

Electron is built on two major upstream projects: Chromium and Node.js. Each of these projects has several of their own dependencies, too. We try our best to use these dependencies exactly as they are but sometimes we can't achieve our goals without patching those upstream dependencies to fit our use cases.

Every patch in Electron is a maintenance burden. When upstream code changes, patches can break—sometimes without even a patch conflict or a compilation error. It's an ongoing effort to keep our patch set up-to-date and effective. So we strive to keep our patch count at a minimum. To that end, every patch must describe its reason for existence in its commit message. That reason must be one of the following:

In general, all the upstream projects we work with are friendly folks and are often happy to accept refactorings that allow the code in question to be compatible with both Electron and the upstream project. (See e.g. this change in Chromium, which allowed us to remove a patch that did the same thing, or this change in Node, which was a no-op for Node but fixed a bug in Electron.) We should aim to upstream changes whenever we can, and avoid indefinite-lifetime patches.

If you find yourself in the unfortunate position of having to make a change which can only be made through patching an upstream project, you'll need to know how to manage patches in Electron.

All patches to upstream projects in Electron are contained in the patches/ directory. Each subdirectory of patches/ contains several patch files, along with a .patches file which lists the order in which the patches should be applied. Think of these files as making up a series of git commits that are applied on top of the upstream project after we check it out.

To help manage these patch sets, we provide two tools: git-import-patches and git-export-patches. git-import-patches imports a set of patch files into a git repository by applying each patch in the correct order and creating a commit for each one. git-export-patches does the reverse; it exports a series of git commits in a repository into a set of files in a directory and an accompanying .patches file.

Side note: the reason we use a .patches file to maintain the order of applied patches, rather than prepending a number like 001- to each file, is because it reduces conflicts related to patch ordering. It prevents the situation where two PRs both add a patch at the end of the series with the same numbering and end up both getting merged resulting in a duplicate identifier, and it also reduces churn when a patch is added or deleted in the middle of the series.

git-export-patches ignores any uncommitted files, so you must create a commit if you want your changes to be exported. The subject line of the commit message will be used to derive the patch file name, and the body of the commit message should include the reason for the patch's existence.

Re-exporting patches will sometimes cause shasums in unrelated patches to change. This is generally harmless and can be ignored (but go ahead and add those changes to your PR, it'll stop them from showing up for other people).

Note that the ^ symbol can cause trouble on Windows. The workaround is to either quote it "[COMMIT_SHA]^" or avoid it [COMMIT_SHA]~1.

Note that git-import-patches will mark the commit that was HEAD when it was run as refs/patches/upstream-head. This lets you keep track of which commits are from Electron patches (those that come after refs/patches/upstream-head) and which commits are in upstream (those before refs/patches/upstream-head).

When updating an upstream dependency, patches may fail to apply cleanly. Often, the conflict can be resolved automatically by git with a 3-way merge. You can instruct git-import-patches to use the 3-way merge algorithm by passing the -3 argument:

If git-import-patches -3 encounters a merge conflict that it can't resolve automatically, it will pause and allow you to resolve the conflict manually. Once you have resolved the conflict, git add the resolved files and continue to apply the rest of the patches by running git am --continue.

**Examples:**

Example 1 (r):
```r
patches├── config.json   <-- this describes which patchset directory is applied to what project├── chromium│   ├── .patches│   ├── accelerator.patch│   ├── add_contentgpuclient_precreatemessageloop_callback.patch│   ⋮├── node│   ├── .patches│   ├── add_openssl_is_boringssl_guard_to_oaep_hash_check.patch│   ├── build_add_gn_build_files.patch│   ⋮⋮
```

Example 2 (bash):
```bash
$ cd src/third_party/electron_node$ vim some/code/file.cc$ git commit$ ../../electron/script/git-export-patches -o ../../electron/patches/node
```

Example 3 (bash):
```bash
$ cd src/v8$ vim some/code/file.cc$ git log# Find the commit sha of the patch you want to edit.$ git commit --fixup [COMMIT_SHA]$ git rebase --autosquash -i [COMMIT_SHA]^$ ../electron/script/git-export-patches -o ../electron/patches/v8
```

Example 4 (bash):
```bash
$ vim src/electron/patches/node/.patches# Delete the line with the name of the patch you want to remove$ cd src/third_party/electron_node$ git reset --hard refs/patches/upstream-head$ ../../electron/script/git-import-patches ../../electron/patches/node$ ../../electron/script/git-export-patches -o ../../electron/patches/node
```

---

## Electron Debugging

**URL:** https://www.electronjs.org/docs/latest/development/debugging

**Contents:**
- Electron Debugging
- Generic Debugging​
- Printing Stacktraces​
- Breakpoint Debugging​
- Platform-Specific Debugging​
- Debugging with the Symbol Server​

There are many different approaches to debugging issues and bugs in Electron, many of which are platform specific.

Some of the more common approaches are outlined below.

Chromium contains logging macros which can aid debugging by printing information to console in C++ and Objective-C++.

You might use this to print out variable values, function names, and line numbers, amongst other things.

There are also different levels of logging severity: INFO, WARN, and ERROR.

See logging.h in Chromium's source tree for more information and examples.

Chromium contains a helper to print stack traces to console without interrupting the program.

This will allow you to observe call chains and identify potential issue areas.

Note that this will increase the size of the build significantly, taking up around 50G of disk space

Write the following file to electron/.git/info/exclude/debug.gn

Now you can use LLDB for breakpoint debugging.

Debug symbols allow you to have better debugging sessions. They have information about the functions contained in executables and dynamic libraries and provide you with information to get clean call stacks. A Symbol Server allows the debugger to load the correct symbols, binaries and sources automatically without forcing users to download large debugging files.

For more information about how to set up a symbol server for Electron, see debugging with a symbol server.

**Examples:**

Example 1 (cpp):
```cpp
LOG(INFO) << "bitmap.width(): " << bitmap.width();LOG(INFO, bitmap.width() > 10) << "bitmap.width() is greater than 10!";
```

Example 2 (cpp):
```cpp
#include "base/debug/stack_trace.h"...base::debug::StackTrace().Print();
```

Example 3 (unknown):
```unknown
import("//electron/build/args/testing.gn")is_debug = truesymbol_level = 2forbid_non_component_debug_builds = false
```

Example 4 (bash):
```bash
$ gn gen out/Debug --args="import(\"//electron/.git/info/exclude/debug.gn\") $GN_EXTRA_ARGS"$ ninja -C out/Debug electron
```

---

## Build Instructions (macOS)

**URL:** https://www.electronjs.org/docs/latest/development/build-instructions-macos

**Contents:**
- Build Instructions (macOS)
- Prerequisites​
  - Arm64-specific prerequisites​
- Building Electron​
- Troubleshooting​
  - Xcode "incompatible architecture" errors (MacOS arm64-specific)​
  - Certificates fail to verify​

Follow the guidelines below for building Electron itself on macOS, for the purposes of creating custom Electron binaries. For bundling and distributing your app code with the prebuilt Electron binaries, see the application distribution guide.

See Build Instructions: GN.

If both Xcode and Xcode command line tools are installed ($ xcode -select --install, or directly download the correct version here), but the stack trace says otherwise like so:

If you are on arm64 architecture, the build script may be pointing to the wrong Xcode version (11.x.y doesn't support arm64). Navigate to /Users/<user>/.electron_build_tools/third_party/Xcode/ and rename Xcode-13.3.0.app to Xcode.app to ensure the right Xcode version is used.

installing certifi will fix the following error:

This issue has to do with Python 3.6 using its own copy of OpenSSL in lieu of the deprecated Apple-supplied OpenSSL libraries. certifi adds a curated bundle of default root certificates. This issue is documented in the Electron repo here. Further information about this issue can be found here and here.

**Examples:**

Example 1 (yaml):
```yaml
xcrun: error: unable to load libxcrun(dlopen(/Users/<user>/.electron_build_tools/third_party/Xcode/Xcode.app/Contents/Developer/usr/lib/libxcrun.dylib (http://xcode.app/Contents/Developer/usr/lib/libxcrun.dylib), 0x0005): tried: '/Users/<user>/.electron_build_tools/third_party/Xcode/Xcode.app/Contents/Developer/usr/lib/libxcrun.dylib (http://xcode.app/Contents/Developer/usr/lib/libxcrun.dylib)' (mach-o file, but is an incompatible architecture (have (x86_64), need (arm64e))), '/Users/<user>/.electron_build_tools/third_party/Xcode/Xcode-11.1.0.app/Contents/Developer/usr/lib/libxcrun.dylib (http://xcode-11.1.0.app/Contents/Developer/usr/lib/libxcrun.dylib)' (mach-o file, but is an incompatible architecture (have (x86_64), need (arm64e)))).`
```

Example 2 (jsx):
```jsx
________ running 'python3 src/tools/clang/scripts/update.py' in '/Users/<user>/electron'Downloading https://commondatastorage.googleapis.com/chromium-browser-clang/Mac_arm64/clang-llvmorg-15-init-15652-g89a99ec9-1.tgz<urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:997)>Retrying in 5 s ...Downloading https://commondatastorage.googleapis.com/chromium-browser-clang/Mac_arm64/clang-llvmorg-15-init-15652-g89a99ec9-1.tgz<urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:997)>Retrying in 10 s ...Downloading https://commondatastorage.googleapis.com/chromium-browser-clang/Mac_arm64/clang-llvmorg-15-init-15652-g89a99ec9-1.tgz<urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:997)>Retrying in 20 s ...
```

---

## Testing

**URL:** https://www.electronjs.org/docs/latest/development/testing

**Contents:**
- Testing
- Linting​
- Unit Tests​
- Node.js Smoke Tests​
  - Testing on Windows 10 devices​
    - Extra steps to run the unit test:​
    - Missing fonts​
    - Pixel measurements​

We aim to keep the code coverage of Electron high. We ask that all pull request not only pass all existing tests, but ideally also add new tests to cover changed code and new scenarios. Ensuring that we capture as many code paths and use cases of Electron as possible ensures that we all ship apps with fewer bugs.

This repository comes with linting rules for both JavaScript and C++ – as well as unit and integration tests. To learn more about Electron's coding style, please see the coding-style document.

To ensure that your changes are in compliance with the Electron coding style, run npm run lint, which will run a variety of linting checks against your changes depending on which areas of the code they touch.

Many of these checks are included as precommit hooks, so it's likely you error would be caught at commit time.

If you are not using build-tools, ensure that the name you have configured for your local build of Electron is one of Testing, Release, Default, or you have set process.env.ELECTRON_OUT_DIR. Without these set, Electron will fail to perform some pre-testing steps.

To run all unit tests, run npm run test. The unit tests are an Electron app (surprise!) that can be found in the spec folder. Note that it has its own package.json and that its dependencies are therefore not defined in the top-level package.json.

To run only specific tests matching a pattern, run npm run test -- -g=PATTERN, replacing the PATTERN with a regex that matches the tests you would like to run. As an example: If you want to run only IPC tests, you would run npm run test -- -g ipc.

If you've made changes that might affect the way Node.js is embedded into Electron, we have a test runner that runs all of the tests from Node.js, using Electron's custom fork of Node.js.

To run all of the Node.js tests:

To run a single Node.js test:

where the argument passed to the runner is the path to the test in the Node.js source tree.

Visual Studio 2019 must be installed.

Node headers have to be compiled for your configuration.

The electron.lib has to be copied as node.lib.

Some Windows 10 devices do not ship with the Meiryo font installed, which may cause a font fallback test to fail. To install Meiryo:

Some tests which rely on precise pixel measurements may not work correctly on devices with Hi-DPI screen settings due to floating point precision errors. To run these tests correctly, make sure the device is set to 100% scaling.

To configure display scaling:

**Examples:**

Example 1 (bash):
```bash
$ node script/node-spec-runner.js
```

Example 2 (bash):
```bash
$ node script/node-spec-runner.js parallel/test-crypto-keygen
```

Example 3 (powershell):
```powershell
ninja -C out\Testing electron:node_headers
```

Example 4 (powershell):
```powershell
cd out\Testingmkdir gen\node_headers\Releasecopy electron.lib gen\node_headers\Release\node.lib
```

---

## Issues In Electron

**URL:** https://www.electronjs.org/docs/latest/development/issues

**Contents:**
- Issues In Electron
- How to Contribute to Issues​
- Asking for General Help​
- Submitting a Bug Report​
- Triaging a Bug Report​
- Resolving a Bug Report​

For any issue, there are fundamentally three ways an individual can contribute:

The Electron website has a list of resources for getting programming help, reporting security issues, contributing, and more. Please use the issue tracker for bugs only!

To submit a bug report:

When opening a new issue in the electron/electron issue tracker, users will be presented with a template that should be filled in.

If you believe that you have found a bug in Electron, please fill out the template to the best of your ability.

The two most important pieces of information needed to evaluate the report are a description of the bug and a simple test case to recreate it. It is easier to fix a bug if it can be reproduced.

See How to create a Minimal, Complete, and Verifiable example.

It's common for open issues to involve discussion. Some contributors may have differing opinions, including whether the behavior is a bug or feature. This discussion is part of the process and should be kept focused, helpful, and professional.

Terse responses that provide neither additional context nor supporting detail are not helpful or professional. To many, such responses are annoying and unfriendly.

Contributors are encouraged to solve issues collaboratively and help one another make progress. If you encounter an issue that you feel is invalid, or which contains incorrect information, explain why you feel that way with additional supporting context, and be willing to be convinced that you may be wrong. By doing so, we can often reach the correct outcome faster.

Most issues are resolved by opening a pull request. The process for opening and reviewing a pull request is similar to that of opening and triaging issues, but carries with it a necessary review and approval workflow that ensures that the proposed changes meet the minimal quality and functional guidelines of the Electron project.

---

## Creating a New Electron Browser Module

**URL:** https://www.electronjs.org/docs/latest/development/creating-api

**Contents:**
- Creating a New Electron Browser Module
- Add your files to Electron's project configuration​
- Create API documentation​
- Set up ObjectTemplateBuilder and Wrappable​
- Link your Electron API with Node​
- Expose your API to TypeScript​
  - Export your API as a module​
  - Expose your module to TypeScript​

Welcome to the Electron API guide! If you are unfamiliar with creating a new Electron API module within the browser directory, this guide serves as a checklist for some of the necessary steps that you will need to implement.

This is not a comprehensive end-all guide to creating an Electron Browser API, rather an outline documenting some of the more unintuitive steps.

Electron uses GN as a meta build system to generate files for its compiler, Ninja. This means that in order to tell Electron to compile your code, we have to add your API's code and header file names into filenames.gni.

You will need to append your API file names alphabetically into the appropriate files like so:

Note that the Windows, macOS and Linux array additions are optional and should only be added if your API has specific platform implementations.

Type definitions are generated by Electron using @electron/docs-parser and @electron/typescript-definitions. This step is necessary to ensure consistency across Electron's API documentation. This means that for your API type definition to appear in the electron.d.ts file, we must create a .md file. Examples can be found in this folder.

Electron constructs its modules using object_template_builder.

wrappable is a base class for C++ objects that have corresponding v8 wrapper objects.

Here is a basic example of code that you may need to add, in order to incorporate object_template_builder and wrappable into your API. For further reference, you can find more implementations here.

In your api_name.h file:

In your api_name.cc file:

In the typings/internal-ambient.d.ts file, we need to append a new property onto the Process interface like so:

At the very bottom of your api_name.cc file:

In your shell/common/node_bindings.cc file, add your node binding name to Electron's built-in modules.

More technical details on how Node links with Electron can be found on our blog.

We will need to create a new TypeScript file in the path that follows:

"lib/browser/api/{electron_browser_{api_name}}.ts"

An example of the contents of this file can be found here.

Add your module to the module list found at "lib/browser/api/module-list.ts" like so:

**Examples:**

Example 1 (cpp):
```cpp
lib_sources = [    "path/to/api/api_name.cc",    "path/to/api/api_name.h",]lib_sources_mac = [    "path/to/api/api_name_mac.h",    "path/to/api/api_name_mac.mm",]lib_sources_win = [    "path/to/api/api_name_win.cc",    "path/to/api/api_name_win.h",]lib_sources_linux = [    "path/to/api/api_name_linux.cc",    "path/to/api/api_name_linux.h",]
```

Example 2 (cpp):
```cpp
#ifndef ELECTRON_SHELL_BROWSER_API_ELECTRON_API_{API_NAME}_H_#define ELECTRON_SHELL_BROWSER_API_ELECTRON_API_{API_NAME}_H_#include "gin/handle.h"#include "gin/wrappable.h"namespace electron {namespace api {class ApiName : public gin::DeprecatedWrappable<ApiName>  { public:  static gin::Handle<ApiName> Create(v8::Isolate* isolate);  // gin::Wrappable  static gin::DeprecatedWrapperInfo kWrapperInfo;  gin::ObjectTemplateBuilder GetObjectTemplateBuilder(      v8::Isolate* isolate) override;  const char* GetTypeName() override;} // namespace api} // namespace electron
```

Example 3 (cpp):
```cpp
#include "shell/browser/api/electron_api_safe_storage.h"#include "shell/browser/browser.h"#include "shell/common/gin_converters/base_converter.h"#include "shell/common/gin_converters/callback_converter.h"#include "shell/common/gin_helper/dictionary.h"#include "shell/common/gin_helper/object_template_builder.h"#include "shell/common/node_includes.h"#include "shell/common/platform_util.h"namespace electron {namespace api {gin::DeprecatedWrapperInfo ApiName::kWrapperInfo = {gin::kEmbedderNativeGin};gin::ObjectTemplateBuilder ApiName::GetObjectTemplateBuilder(    v8::Isolate* isolate) {  return gin::ObjectTemplateBuilder(isolate)      .SetMethod("methodName", &ApiName::methodName);}const char* ApiName::GetTypeName() {  return "ApiName";}// staticgin::Handle<ApiName> ApiName::Create(v8::Isolate* isolate) {  return gin::CreateHandle(isolate, new ApiName());}} // namespace api} // namespace electronnamespace {void Initialize(v8::Local<v8::Object> exports,                v8::Local<v8::Value> unused,                v8::Local<v8::Context> context,                void* priv) {  v8::Isolate* const isolate = v8::Isolate::GetCurrent();  gin_helper::Dictionary dict(isolate, exports);  dict.Set("apiName", electron::api::ApiName::Create(isolate));}}  // namespace
```

Example 4 (typescript):
```typescript
interface Process {    _linkedBinding(name: 'electron_browser_{api_name}'): Electron.ApiName;}
```

---

## V8 Development

**URL:** https://www.electronjs.org/docs/latest/development/v8-development

**Contents:**
- V8 Development

A collection of resources for learning and using V8

See also Chromium Development

---

## Build Instructions

**URL:** https://www.electronjs.org/docs/latest/development/build-instructions-gn

**Contents:**
- Build Instructions
- Platform prerequisites​
- Build Tools​
- GN Files​
- GN prerequisites​
  - Setting up the git cache​
- Getting the code​
  - A note on pulling/pushing​
- Building​
  - Packaging​

Follow the guidelines below for building Electron itself, for the purposes of creating custom Electron binaries. For bundling and distributing your app code with the prebuilt Electron binaries, see the application distribution guide.

Check the build prerequisites for your platform before proceeding

Electron's Build Tools automate much of the setup for compiling Electron from source with different configurations and build targets. If you wish to set up the environment manually, the instructions are listed below.

Electron uses GN for project generation and ninja for building. Project configurations can be found in the .gn and .gni files.

The following gn files contain the main rules for building Electron:

You'll need to install depot_tools, the toolset used for fetching Chromium and its dependencies.

Also, on Windows, you'll need to set the environment variable DEPOT_TOOLS_WIN_TOOLCHAIN=0. To do so, open Control Panel → System and Security → System → Advanced system settings and add a system variable DEPOT_TOOLS_WIN_TOOLCHAIN with value 0. This tells depot_tools to use your locally installed version of Visual Studio (by default, depot_tools will try to download a Google-internal version that only Googlers have access to).

If you plan on checking out Electron more than once (for example, to have multiple parallel directories checked out to different branches), using the git cache will speed up subsequent calls to gclient. To do this, set a GIT_CACHE_PATH environment variable:

Instead of https://github.com/electron/electron, you can use your own fork here (something like https://github.com/<username>/electron).

If you intend to git pull or git push from the official electron repository in the future, you now need to update the respective folder's origin URLs.

📝 gclient works by checking a file called DEPS inside the src/electron folder for dependencies (like Chromium or Node.js). Running gclient sync -f ensures that all dependencies required to build Electron match that file.

So, in order to pull, you'd run the following commands:

Set the environment variable for chromium build tools

To generate Testing build config of Electron:

To generate Release build config of Electron:

This will generate a out/Testing or out/Release build directory under src/ with the testing or release build depending upon the configuration passed above. You can replace Testing|Release with another names, but it should be a subdirectory of out.

Also you shouldn't have to run gn gen again—if you want to change the build arguments, you can run gn args out/Testing to bring up an editor. To see the list of available build configuration options, run gn args out/Testing --list.

To build, run ninja with the electron target: Note: This will also take a while and probably heat up your lap.

For the testing configuration:

For the release configuration:

This will build all of what was previously 'libchromiumcontent' (i.e. the content/ directory of chromium and its dependencies, incl. Blink and V8), so it will take a while.

The built executable will be under ./out/Testing:

To package the electron build as a distributable zip file:

To compile for a platform that isn't the same as the one you're building on, set the target_cpu and target_os GN arguments. For example, to compile an x86 target from an x64 host, specify target_cpu = "x86" in gn args.

Not all combinations of source and target CPU/OS are supported by Chromium.

If you test other combinations and find them to work, please update this document :)

See the GN reference for allowable values of target_os and target_cpu.

To cross-compile for Windows on Arm, follow Chromium's guide to get the necessary dependencies, SDK and libraries, then build with ELECTRON_BUILDING_WOA=1 in your environment before running gclient sync.

Or (if using PowerShell):

Next, run gn gen as above with target_cpu="arm64".

To run the tests, you'll first need to build the test modules against the same version of Node.js that was built as part of the build process. To generate build headers for the modules to compile against, run the following under src/ directory.

You can now run the tests.

If you're debugging something, it can be helpful to pass some extra flags to the Electron binary:

It is possible to share the gclient git cache with other machines by exporting it as SMB share on linux, but only one process/machine can be using the cache at a time. The locks created by git-cache script will try to prevent this, but it may not work perfectly in a network.

On Windows, SMBv2 has a directory cache that will cause problems with the git cache script, so it is necessary to disable it by setting the registry key

to 0. More information: https://stackoverflow.com/a/9935126

This can be set quickly in powershell (ran as administrator):

If gclient sync is interrupted the git tree may be left in a bad state, leading to a cryptic message when running gclient sync in the future:

If there are no git conflicts or rebases in src/electron, you may need to abort a git am in src:

This may also happen if you have checked out a branch (as opposed to having a detached head) in electron/src/ or some other dependency’s repository. If that is the case, a git checkout --detach HEAD in the appropriate repository should do the trick.

If you see a prompt for Username for 'https://chrome-internal.googlesource.com': when running gclient sync on Windows, it's probably because the DEPOT_TOOLS_WIN_TOOLCHAIN environment variable is not set to 0. Open Control Panel → System and Security → System → Advanced system settings and add a system variable DEPOT_TOOLS_WIN_TOOLCHAIN with value 0. This tells depot_tools to use your locally installed version of Visual Studio (by default, depot_tools will try to download a Google-internal version that only Googlers have access to).

If e is not recognized despite running npm i -g @electron/build-tools, ie:

We recommend installing Node through nvm. This allows for easier Node version management, and is often a fix for missing e modules.

This could be caused by the local clock time on the machine being off by a small amount. Use time.is to check.

**Examples:**

Example 1 (bash):
```bash
$ export GIT_CACHE_PATH="${HOME}/.git_cache"$ mkdir -p "${GIT_CACHE_PATH}"# This will use about 16G.
```

Example 2 (unknown):
```unknown
$ mkdir electron && cd electron$ gclient config --name "src/electron" --unmanaged https://github.com/electron/electron$ gclient sync --with_branch_heads --with_tags# This will take a while, go get a coffee.
```

Example 3 (powershell):
```powershell
$ cd src/electron$ git remote remove origin$ git remote add origin https://github.com/electron/electron$ git checkout main$ git branch --set-upstream-to=origin/main$ cd -
```

Example 4 (unknown):
```unknown
$ cd src/electron$ git pull$ gclient sync -f
```

---

## Debugging with XCode

**URL:** https://www.electronjs.org/docs/latest/development/debugging-with-xcode

**Contents:**
- Debugging with XCode
- Debugging with XCode​
  - Generate xcode project for debugging sources (cannot build code from xcode)​
  - Debugging and breakpoints​

Run gn gen with the --ide=xcode argument.

This will generate the electron.ninja.xcworkspace. You will have to open this workspace to set breakpoints and inspect.

See gn help gen for more information on generating IDE projects with GN.

Launch Electron app after build. You can now open the xcode workspace created above and attach to the Electron process through the Debug > Attach To Process > Electron debug menu. [Note: If you want to debug the renderer process, you need to attach to the Electron Helper as well.]

You can now set breakpoints in any of the indexed files. However, you will not be able to set breakpoints directly in the Chromium source. To set break points in the Chromium source, you can choose Debug > Breakpoints > Create Symbolic Breakpoint and set any function name as the symbol. This will set the breakpoint for all functions with that name, from all the classes if there are more than one. You can also do this step of setting break points prior to attaching the debugger, however, actual breakpoints for symbolic breakpoint functions may not show up until the debugger is attached to the app.

**Examples:**

Example 1 (unknown):
```unknown
$ gn gen out/Testing --ide=xcode
```

---

## Reclient

**URL:** https://www.electronjs.org/docs/latest/development/reclient

**Contents:**
- Reclient
- Enabling Reclient​
- Building with Reclient​
- Access​
- Support​

Reclient integrates with an existing build system to enable remote execution and caching of build actions.

Electron has a deployment of a reclient compatible RBE Backend that is available to all Electron Maintainers. See the Access section below for details on authentication. Non-maintainers will not have access to the cluster, but can sign in to receive a Cache Only token that gives access to the cache-only CAS backend. Using this should result in significantly faster build times .

Currently the only supported way to use Reclient is to use our Build Tools. Reclient configuration is automatically included when you set up build-tools.

If you have an existing config, you can just set "reclient": "remote_exec" in your config file.

When you are using Reclient, you can run autoninja with a substantially higher j value than would normally be supported by your machine.

Please do not set a value higher than 200. The RBE system is monitored. Users found to be abusing it with unreasonable concurrency will be deactivated.

If you're using build-tools, appropriate -j values will automatically be used for you.

For security and cost reasons, access to Electron's RBE backend is currently restricted to Electron Maintainers. If you want access, please head to #access-requests in Slack and ping @infra-wg to ask for it. Please be aware that being a maintainer does not automatically grant access. Access is determined on a case-by-case basis.

We do not provide support for usage of Reclient. Issues raised asking for help / having issues will probably be closed without much reason. We do not have the capacity to handle that kind of support.

**Examples:**

Example 1 (bash):
```bash
autoninja -C out/Testing electron -j 200
```

---

## Setting Up Symbol Server in Debugger

**URL:** https://www.electronjs.org/docs/latest/development/debugging-with-symbol-server

**Contents:**
- Setting Up Symbol Server in Debugger
- Using the Symbol Server in Windbg​
- Using the symbol server in Visual Studio​
- Troubleshooting: Symbols will not load​

Debug symbols allow you to have better debugging sessions. They have information about the functions contained in executables and dynamic libraries and provide you with information to get clean call stacks. A Symbol Server allows the debugger to load the correct symbols, binaries and sources automatically without forcing users to download large debugging files. The server functions like Microsoft's symbol server so the documentation there can be useful.

Note that because released Electron builds are heavily optimized, debugging is not always easy. The debugger will not be able to show you the content of all variables and the execution path can seem strange because of inlining, tail calls, and other compiler optimizations. The only workaround is to build an unoptimized local build.

The official symbol server URL for Electron is https://symbols.electronjs.org. You cannot visit this URL directly, you must add it to the symbol path of your debugging tool. In the examples below, a local cache directory is used to avoid repeatedly fetching the PDB from the server. Replace c:\code\symbols with an appropriate cache directory on your machine.

The Windbg symbol path is configured with a string value delimited with asterisk characters. To use only the Electron symbol server, add the following entry to your symbol path (Note: you can replace c:\code\symbols with any writable directory on your computer, if you'd prefer a different location for downloaded symbols):

Set this string as _NT_SYMBOL_PATH in the environment, using the Windbg menus, or by typing the .sympath command. If you would like to get symbols from Microsoft's symbol server as well, you should list that first:

Type the following commands in Windbg to print why symbols are not loading:

**Examples:**

Example 1 (powershell):
```powershell
SRV*c:\code\symbols\*https://symbols.electronjs.org
```

Example 2 (powershell):
```powershell
SRV*c:\code\symbols\*https://msdl.microsoft.com/download/symbols;SRV*c:\code\symbols\*https://symbols.electronjs.org
```

Example 3 (powershell):
```powershell
> !sym noisy> .reload /f electron.exe
```

---

## Debugging on Windows

**URL:** https://www.electronjs.org/docs/latest/development/debugging-on-windows

**Contents:**
- Debugging on Windows
- Requirements​
- Attaching to and Debugging Electron​
  - Setting Breakpoints​
  - Attaching​
  - Which Process Should I Attach to?​
- Using ProcMon to Observe a Process​
- Using WinDbg​

If you experience crashes or issues in Electron that you believe are not caused by your JavaScript application, but instead by Electron itself, debugging can be a little bit tricky, especially for developers not used to native/C++ debugging. However, using Visual Studio, Electron's hosted Symbol Server, and the Electron source code, you can enable step-through debugging with breakpoints inside Electron's source code.

See also: There's a wealth of information on debugging Chromium, much of which also applies to Electron, on the Chromium developers site: Debugging Chromium on Windows.

A debug build of Electron: The easiest way is usually building it yourself, using the tools and prerequisites listed in the build instructions for Windows. While you can attach to and debug Electron as you can download it directly, you will find that it is heavily optimized, making debugging substantially more difficult: The debugger will not be able to show you the content of all variables and the execution path can seem strange because of inlining, tail calls, and other compiler optimizations.

Visual Studio with C++ Tools: The free community editions of Visual Studio 2013 and Visual Studio 2015 both work. Once installed, configure Visual Studio to use Electron's Symbol server. It will enable Visual Studio to gain a better understanding of what happens inside Electron, making it easier to present variables in a human-readable format.

ProcMon: The free SysInternals tool allows you to inspect a processes parameters, file handles, and registry operations.

To start a debugging session, open up PowerShell/CMD and execute your debug build of Electron, using the application to open as a parameter.

Then, open up Visual Studio. Electron is not built with Visual Studio and hence does not contain a project file - you can however open up the source code files "As File", meaning that Visual Studio will open them up by themselves. You can still set breakpoints - Visual Studio will automatically figure out that the source code matches the code running in the attached process and break accordingly.

Relevant code files can be found in ./shell/.

You can attach the Visual Studio debugger to a running process on a local or remote computer. After the process is running, click Debug / Attach to Process (or press CTRL+ALT+P) to open the "Attach to Process" dialog box. You can use this capability to debug apps that are running on a local or remote computer, debug multiple processes simultaneously.

If Electron is running under a different user account, select the Show processes from all users check box. Notice that depending on how many BrowserWindows your app opened, you will see multiple processes. A typical one-window app will result in Visual Studio presenting you with two Electron.exe entries - one for the main process and one for the renderer process. Since the list only gives you names, there's currently no reliable way of figuring out which is which.

Code executed within the main process (that is, code found in or eventually run by your main JavaScript file) will run inside the main process, while other code will execute inside its respective renderer process.

You can be attached to multiple programs when you are debugging, but only one program is active in the debugger at any time. You can set the active program in the Debug Location toolbar or the Processes window.

While Visual Studio is fantastic for inspecting specific code paths, ProcMon's strength is really in observing everything your application is doing with the operating system - it captures File, Registry, Network, Process, and Profiling details of processes. It attempts to log all events occurring and can be quite overwhelming, but if you seek to understand what and how your application is doing to the operating system, it can be a valuable resource.

For an introduction to ProcMon's basic and advanced debugging features, go check out this video tutorial provided by Microsoft.

It's possible to debug crashes and issues in the Renderer process with WinDbg.

To attach to a debug a process with WinDbg:

**Examples:**

Example 1 (powershell):
```powershell
$ ./out/Testing/electron.exe ~/my-electron-app/
```

---

## Build Instructions (Linux)

**URL:** https://www.electronjs.org/docs/latest/development/build-instructions-linux

**Contents:**
- Build Instructions (Linux)
- Prerequisites​
  - Cross compilation​
- Building​
- Troubleshooting​
  - Error While Loading Shared Libraries: libtinfo.so.5​
- Advanced topics​
  - Using system clang instead of downloaded clang binaries​
  - Using compilers other than clang​

Follow the guidelines below for building Electron itself on Linux, for the purposes of creating custom Electron binaries. For bundling and distributing your app code with the prebuilt Electron binaries, see the application distribution guide.

Due to Electron's dependency on Chromium, prerequisites and dependencies for Electron change over time. Chromium's documentation on building on Linux has up to date information for building Chromium on Linux. This documentation can generally be followed for building Electron on Linux as well.

Additionally, Electron's Linux dependency installer can be referenced to get the current dependencies that Electron requires in addition to what Chromium installs via build/install-deps.sh.

If you want to build for an arm target, you can use Electron's Linux dependency installer to install the additional dependencies by passing the --arm argument:

And to cross-compile for arm or targets, you should pass the target_cpu parameter to gn gen:

See Build Instructions: GN

Prebuilt clang will try to link to libtinfo.so.5. Depending on the host architecture, symlink to appropriate libncurses:

The default building configuration is targeted for major desktop Linux distributions. To build for a specific distribution or device, the following information may help you.

By default Electron is built with prebuilt clang binaries provided by the Chromium project. If for some reason you want to build with the clang installed in your system, you can specify the clang_base_path argument in the GN args.

For example if you installed clang under /usr/local/bin/clang:

Building Electron with compilers other than clang is not supported.

**Examples:**

Example 1 (unknown):
```unknown
$ sudo install-deps.sh --arm
```

Example 2 (unknown):
```unknown
$ gn gen out/Testing --args='import(...) target_cpu="arm"'
```

Example 3 (unknown):
```unknown
$ sudo ln -s /usr/lib/libncurses.so.5 /usr/lib/libtinfo.so.5
```

Example 4 (unknown):
```unknown
$ gn gen out/Testing --args='import("//electron/build/args/testing.gn") clang_base_path = "/usr/local/bin"'
```

---

## Electron Documentation Style Guide

**URL:** https://www.electronjs.org/docs/latest/development/style-guide

**Contents:**
- Electron Documentation Style Guide
- Headings​
- Markdown rules​
- Picking words​
- API references​
  - Title and description​
  - Module methods and events​
  - Classes​
  - Methods and their arguments​
    - Heading level​

These are the guidelines for writing Electron documentation.

Using Quick Start as example:

For API references, there are exceptions to this rule.

This repository uses the markdownlint package to enforce consistent Markdown styling. For the exact rules, see the .markdownlint.json file in the root folder.

There are a few style guidelines that aren't covered by the linter rules:

The following rules only apply to the documentation of APIs.

Each module's API doc must use the actual object name returned by require('electron') as its title (such as BrowserWindow, autoUpdater, and session).

Directly under the page title, add a one-line description of the module as a markdown quote (beginning with >).

Using the session module as an example:

For modules that are not classes, their methods and events must be listed under the ## Methods and ## Events chapters.

Using autoUpdater as an example:

Using the Session and Cookies classes as an example:

The methods chapter must be in the following form:

The heading can be ### or ####-levels depending on whether the method belongs to a module or a class.

For modules, the objectName is the module's name. For classes, it must be the name of the instance of the class, and must not be the same as the module's name.

For example, the methods of the Session class under the session module must use ses as the objectName.

Optional arguments are notated by square brackets [] surrounding the optional argument as well as the comma required if this optional argument follows another argument:

More detailed information on each of the arguments is noted in an unordered list below the method. The type of argument is notated by either JavaScript primitives (e.g. string, Promise, or Object), a custom API structure like Electron's Cookie, or the wildcard any.

If the argument is of type Array, use [] shorthand with the type of value inside the array (for example,any[] or string[]).

If the argument is of type Promise, parametrize the type with what the promise resolves to (for example, Promise<void> or Promise<string>).

If an argument can be of multiple types, separate the types with |.

The description for Function type arguments should make it clear how it may be called and list the types of the parameters that will be passed to it.

If an argument or a method is unique to certain platforms, those platforms are denoted using a space-delimited italicized list following the datatype. Values can be macOS, Windows or Linux.

The events chapter must be in following form:

The heading can be ### or ####-levels depending on whether the event belongs to a module or a class.

The arguments of an event follow the same rules as methods.

The properties chapter must be in following form:

The heading can be ### or ####-levels depending on whether the property belongs to a module or a class.

An "API History" block is a YAML code block encapsulated by an HTML comment that should be placed directly after the Markdown header for a class or method, like so:

It should adhere to the API History JSON Schema (api-history.schema.json) which you can find in the docs folder. The API History Schema RFC includes example usage and detailed explanations for each aspect of the schema.

The purpose of the API History block is to describe when/where/how/why an API was:

Each API change listed in the block should include a link to the PR where that change was made along with an optional short description of the change. If applicable, include the heading id for that change from the breaking changes documentation.

The API History linting script (lint:api-history) validates API History blocks in the Electron documentation against the schema and performs some other checks. You can look at its tests for more details.

There are a few style guidelines that aren't covered by the linting script:

Always adhere to this format:

Generally, you should place the API History block directly after the Markdown header for a class or method that was changed. However, there are some instances where this is ambiguous:

Sometimes a breaking change doesn't relate to any of the existing APIs. In this case, it is ok not to add API History anywhere.

Sometimes a breaking change involves multiple APIs. In this case, place the API History block under the top-level Markdown header for each of the involved APIs.

Notice how an API History block wasn't added under:

since that function wasn't changed, only how it may be used:

**Examples:**

Example 1 (markdown):
```markdown
# Quick Start...## Main process...## Renderer process...## Run your app...### Run as a distribution...### Manually downloaded Electron binary...
```

Example 2 (markdown):
```markdown
# session> Manage browser sessions, cookies, cache, proxy settings, etc.
```

Example 3 (markdown):
```markdown
# autoUpdater## Events### Event: 'error'## Methods### `autoUpdater.setFeedURL(url[, requestHeaders])`
```

Example 4 (markdown):
```markdown
# session## Methods### session.fromPartition(partition)## Static Properties### session.defaultSession## Class: Session### Instance Events#### Event: 'will-download'### Instance Methods#### `ses.getCacheSize()`### Instance Properties#### `ses.cookies`## Class: Cookies### Instance Methods#### `cookies.get(filter, callback)`
```

---

## Build Instructions (Windows)

**URL:** https://www.electronjs.org/docs/latest/development/build-instructions-windows

**Contents:**
- Build Instructions (Windows)
- Prerequisites​
- Exclude source tree from Windows Security​
- Building​
- 32bit Build​
- Visual Studio project​
- Troubleshooting​
  - Command xxxx not found​
  - Fatal internal compiler error: C1001​
  - LNK1181: cannot open input file 'kernel32.lib'​

Follow the guidelines below for building Electron itself on Windows, for the purposes of creating custom Electron binaries. For bundling and distributing your app code with the prebuilt Electron binaries, see the application distribution guide.

If you don't currently have a Windows installation, developer.microsoft.com has timebombed versions of Windows that you can use to build Electron.

Building Electron is done entirely with command-line scripts and cannot be done with Visual Studio. You can develop Electron with any editor but support for building with Visual Studio will come in the future.

Even though Visual Studio is not used for building, it's still required because we need the build toolchains it provides.

Windows Security doesn't like one of the files in the Chromium source code (see https://crbug.com/441184), so it will constantly delete it, causing gclient sync issues. You can exclude the source tree from being monitored by Windows Security by following these instructions.

See Build Instructions: GN

To build for the 32bit target, you need to pass target_cpu = "x86" as a GN arg. You can build the 32bit target alongside the 64bit target by using a different output directory for GN, e.g. out/Release-x86, with different arguments.

The other building steps are exactly the same.

To generate a Visual Studio project, you can pass the --ide=vs2017 parameter to gn gen:

If you encountered an error like Command xxxx not found, you may try to use the VS2015 Command Prompt console to execute the build scripts.

Make sure you have the latest Visual Studio update installed.

Try reinstalling 32bit Node.js.

Creating that directory should fix the problem:

You may get this error if you are using Git Bash for building, you should use PowerShell or VS2015 Command Prompt instead.

node.js has some extremely long pathnames, and by default git on windows doesn't handle long pathnames correctly (even though windows supports them). This should fix it:

This can happen during build, when Debugging Tools for Windows has been installed with Windows Driver Kit. Uninstall Windows Driver Kit and install Debugging Tools with steps described above.

This bug is a "feature" of Windows' command prompt. It happens when clicking inside the prompt window with QuickEdit enabled and is intended to allow selecting and copying output text easily. Since each accidental click will pause the build process, you might want to disable this feature in the command prompt properties.

**Examples:**

Example 1 (powershell):
```powershell
$ gn gen out/Release-x86 --args="import(\"//electron/build/args/release.gn\") target_cpu=\"x86\""
```

Example 2 (powershell):
```powershell
$ gn gen out/Testing --ide=vs2017
```

Example 3 (powershell):
```powershell
$ mkdir ~\AppData\Roaming\npm
```

Example 4 (unknown):
```unknown
$ git config --system core.longpaths true
```

---

## Chromium Development

**URL:** https://www.electronjs.org/docs/latest/development/chromium-development

**Contents:**
- Chromium Development
- Contributing to Chromium​
- Resources for Chromium Development​
  - Code Resources​
  - Informational Resources​
- Social Links​

A collection of resources for learning about Chromium and tracking its development.

See also V8 Development

Checking Out and Building

Contributing - This document outlines the process of getting a code change merged to the Chromium source tree.

---

## Source Code Directory Structure

**URL:** https://www.electronjs.org/docs/latest/development/source-code-directory-structure

**Contents:**
- Source Code Directory Structure
- Structure of Source Code​
- Structure of Other Directories​

The source code of Electron is separated into a few parts, mostly following Chromium on the separation conventions.

You may need to become familiar with Chromium's multi-process architecture to understand the source code better.

**Examples:**

Example 1 (csharp):
```csharp
Electron├── build/ - Build configuration files needed to build with GN.├── buildflags/ - Determines the set of features that can be conditionally built.├── chromium_src/ - Source code copied from Chromium that isn't part of the content layer.├── default_app/ - A default app run when Electron is started without|                  providing a consumer app.├── docs/ - Electron's documentation.|   ├── api/ - Documentation for Electron's externally-facing modules and APIs.|   ├── development/ - Documentation to aid in developing for and with Electron.|   ├── fiddles/ - A set of code snippets one can run in Electron Fiddle.|   ├── images/ - Images used in documentation.|   └── tutorial/ - Tutorial documents for various aspects of Electron.├── lib/ - JavaScript/TypeScript source code.|   ├── browser/ - Main process initialization code.|   |   ├── api/ - API implementation for main process modules.|   |   └── remote/ - Code related to the remote module as it is|   |                 used in the main process.|   ├── common/ - Relating to logic needed by both main and renderer processes.|   |   └── api/ - API implementation for modules that can be used in|   |              both the main and renderer processes|   ├── isolated_renderer/ - Handles creation of isolated renderer processes when|   |                        contextIsolation is enabled.|   ├── renderer/ - Renderer process initialization code.|   |   ├── api/ - API implementation for renderer process modules.|   |   ├── extension/ - Code related to use of Chrome Extensions|   |   |                in Electron's renderer process.|   |   ├── remote/ - Logic that handles use of the remote module in|   |   |             the main process.|   |   └── web-view/ - Logic that handles the use of webviews in the|   |                   renderer process.|   ├── sandboxed_renderer/ - Logic that handles creation of sandboxed renderer|   |   |                     processes.|   |   └── api/ - API implementation for sandboxed renderer processes.|   └── worker/ - Logic that handles proper functionality of Node.js|                 environments in Web Workers.├── patches/ - Patches applied on top of Electron's core dependencies|   |          in order to handle differences between our use cases and|   |          default functionality.|   ├── boringssl/ - Patches applied to Google's fork of OpenSSL, BoringSSL.|   ├── chromium/ - Patches applied to Chromium.|   ├── node/ - Patches applied on top of Node.js.|   └── v8/ - Patches applied on top of Google's V8 engine.├── shell/ - C++ source code.|   ├── app/ - System entry code.|   ├── browser/ - The frontend including the main window, UI, and all of the|   |   |          main process things. This talks to the renderer to manage web|   |   |          pages.|   |   ├── ui/ - Implementation of UI stuff for different platforms.|   |   |   ├── cocoa/ - Cocoa specific source code.|   |   |   ├── win/ - Windows GUI specific source code.|   |   |   └── x/ - X11 specific source code.|   |   ├── api/ - The implementation of the main process APIs.|   |   ├── net/ - Network related code.|   |   ├── mac/ - Mac specific Objective-C source code.|   |   └── resources/ - Icons, platform-dependent files, etc.|   ├── renderer/ - Code that runs in renderer process.|   |   └── api/ - The implementation of renderer process APIs.|   └── common/ - Code that used by both the main and renderer processes,|       |         including some utility functions and code to integrate node's|       |         message loop into Chromium's message loop.|       └── api/ - The implementation of common APIs, and foundations of|                  Electron's built-in modules.├── spec/ - Components of Electron's test suite run in the main process.└── BUILD.gn - Building rules of Electron.
```

Example 2 (unknown):
```unknown
script/ - The set of all scripts Electron runs for a variety of purposes.├── codesign/ - Fakes codesigning for Electron apps; used for testing.├── lib/ - Miscellaneous python utility scripts.└── release/ - Scripts run during Electron's release process.    ├── notes/ - Generates release notes for new Electron versions.    └── uploaders/ - Uploads various release-related files during release.
```

---
