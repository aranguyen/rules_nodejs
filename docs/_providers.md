
Public providers, aspects and helpers that are shipped in the built-in build_bazel_rules_nodejs repository.

Users should not load files under "/internal"


## DeclarationInfo

**USAGE**

<pre>
DeclarationInfo(<a href="#DeclarationInfo-declarations">declarations</a>, <a href="#DeclarationInfo-transitive_declarations">transitive_declarations</a>, <a href="#DeclarationInfo-type_blacklisted_declarations">type_blacklisted_declarations</a>)
</pre>

The DeclarationInfo provider allows JS rules to communicate typing information.
TypeScript's .d.ts files are used as the interop format for describing types.
package.json files are included as well, as TypeScript needs to read the "typings" property.

Do not create DeclarationInfo instances directly, instead use the declaration_info factory function.

Note: historically this was a subset of the string-typed "typescript" provider.


**FIELDS**

<h4 id="DeclarationInfo-declarations">declarations</h4>

 A depset of typings files produced by this rule 
<h4 id="DeclarationInfo-transitive_declarations">transitive_declarations</h4>

 A depset of typings files produced by this rule and all its transitive dependencies.
This prevents needing an aspect in rules that consume the typings, which improves performance. 
<h4 id="DeclarationInfo-type_blacklisted_declarations">type_blacklisted_declarations</h4>

 A depset of .d.ts files that we should not use to infer JSCompiler types (via tsickle) 


## ExternalNpmPackageInfo

**USAGE**

<pre>
ExternalNpmPackageInfo(<a href="#ExternalNpmPackageInfo-direct_sources">direct_sources</a>, <a href="#ExternalNpmPackageInfo-path">path</a>, <a href="#ExternalNpmPackageInfo-sources">sources</a>, <a href="#ExternalNpmPackageInfo-workspace">workspace</a>)
</pre>

Provides information about one or more external npm packages

**FIELDS**

<h4 id="ExternalNpmPackageInfo-direct_sources">direct_sources</h4>

 Depset of direct source files in these external npm package(s) 
<h4 id="ExternalNpmPackageInfo-path">path</h4>

 The local workspace path that these external npm deps should be linked at. If empty, they will be linked at the root. 
<h4 id="ExternalNpmPackageInfo-sources">sources</h4>

 Depset of direct & transitive source files in these external npm package(s) and transitive dependencies 
<h4 id="ExternalNpmPackageInfo-workspace">workspace</h4>

 The workspace name that these external npm package(s) are provided from 


## JSEcmaScriptModuleInfo

**USAGE**

<pre>
JSEcmaScriptModuleInfo(<a href="#JSEcmaScriptModuleInfo-direct_sources">direct_sources</a>, <a href="#JSEcmaScriptModuleInfo-sources">sources</a>)
</pre>

JavaScript files (and sourcemaps) that are intended to be consumed by downstream tooling.

They should use modern syntax and ESModules.
These files should typically be named "foo.mjs"

Historical note: this was the typescript.es6_sources output

**FIELDS**

<h4 id="JSEcmaScriptModuleInfo-direct_sources">direct_sources</h4>

 Depset of direct JavaScript files and sourcemaps 
<h4 id="JSEcmaScriptModuleInfo-sources">sources</h4>

 Depset of direct and transitive JavaScript files and sourcemaps 


## JSModuleInfo

**USAGE**

<pre>
JSModuleInfo(<a href="#JSModuleInfo-direct_sources">direct_sources</a>, <a href="#JSModuleInfo-sources">sources</a>)
</pre>

JavaScript files and sourcemaps.

**FIELDS**

<h4 id="JSModuleInfo-direct_sources">direct_sources</h4>

 Depset of direct JavaScript files and sourcemaps 
<h4 id="JSModuleInfo-sources">sources</h4>

 Depset of direct and transitive JavaScript files and sourcemaps 


## JSNamedModuleInfo

**USAGE**

<pre>
JSNamedModuleInfo(<a href="#JSNamedModuleInfo-direct_sources">direct_sources</a>, <a href="#JSNamedModuleInfo-sources">sources</a>)
</pre>

JavaScript files whose module name is self-contained.

For example named AMD/UMD or goog.module format.
These files can be efficiently served with the concatjs bundler.
These outputs should be named "foo.umd.js"
(note that renaming it from "foo.js" doesn't affect the module id)

Historical note: this was the typescript.es5_sources output.


**FIELDS**

<h4 id="JSNamedModuleInfo-direct_sources">direct_sources</h4>

 Depset of direct JavaScript files and sourcemaps 
<h4 id="JSNamedModuleInfo-sources">sources</h4>

 Depset of direct and transitive JavaScript files and sourcemaps 


## LinkablePackageInfo

**USAGE**

<pre>
LinkablePackageInfo(<a href="#LinkablePackageInfo-files">files</a>, <a href="#LinkablePackageInfo-package_name">package_name</a>, <a href="#LinkablePackageInfo-package_path">package_path</a>, <a href="#LinkablePackageInfo-path">path</a>, <a href="#LinkablePackageInfo-_tslibrary">_tslibrary</a>)
</pre>

The LinkablePackageInfo provider provides information to the linker for linking pkg_npm built packages

**FIELDS**

<h4 id="LinkablePackageInfo-files">files</h4>

 Depset of files in this package (must all be contained within path) 
<h4 id="LinkablePackageInfo-package_name">package_name</h4>

 The package name.

This field is optional. If not set, the target can be made linkable to a package_name with the npm_link rule. 
<h4 id="LinkablePackageInfo-package_path">package_path</h4>

 The directory in the workspace to link to.

If set, link the 1st party dependencies to the node_modules under the package path specified.
If unset, the default is to link to the node_modules root of the workspace. 
<h4 id="LinkablePackageInfo-path">path</h4>

 The path to link to.

Path must be relative to execroot/wksp. It can either an output dir path such as,

`bazel-out/<platform>-<build>/bin/path/to/package` or
`bazel-out/<platform>-<build>/bin/external/external_wksp>/path/to/package`

or a source file path such as,

`path/to/package` or
`external/<external_wksp>/path/to/package` 
<h4 id="LinkablePackageInfo-_tslibrary">_tslibrary</h4>

 For internal use only 


## NodeContextInfo

**USAGE**

<pre>
NodeContextInfo(<a href="#NodeContextInfo-stamp">stamp</a>)
</pre>

Provides data about the build context, like config_setting's

**FIELDS**

<h4 id="NodeContextInfo-stamp">stamp</h4>

 If stamping is enabled 


## NodeRuntimeDepsInfo

**USAGE**

<pre>
NodeRuntimeDepsInfo(<a href="#NodeRuntimeDepsInfo-deps">deps</a>, <a href="#NodeRuntimeDepsInfo-pkgs">pkgs</a>)
</pre>

Stores runtime dependencies of a nodejs_binary or nodejs_test

These are files that need to be found by the node module resolver at runtime.

Historically these files were passed using the Runfiles mechanism.
However runfiles has a big performance penalty of creating a symlink forest
with FS API calls for every file in node_modules.
It also causes there to be separate node_modules trees under each binary. This
prevents user-contributed modules passed as deps[] to a particular action from
being found by node module resolver, which expects everything in one tree.

In node, this resolution is done dynamically by assuming a node_modules
tree will exist on disk, so we assume node actions/binary/test executions will
do the same.


**FIELDS**

<h4 id="NodeRuntimeDepsInfo-deps">deps</h4>

 depset of runtime dependency labels 
<h4 id="NodeRuntimeDepsInfo-pkgs">pkgs</h4>

 list of labels of packages that provide ExternalNpmPackageInfo 


## NpmPackageInfo

**USAGE**

<pre>
NpmPackageInfo(<a href="#NpmPackageInfo-direct_sources">direct_sources</a>, <a href="#NpmPackageInfo-path">path</a>, <a href="#NpmPackageInfo-sources">sources</a>, <a href="#NpmPackageInfo-workspace">workspace</a>)
</pre>

Provides information about one or more external npm packages

**FIELDS**

<h4 id="NpmPackageInfo-direct_sources">direct_sources</h4>

 Depset of direct source files in these external npm package(s) 
<h4 id="NpmPackageInfo-path">path</h4>

 The local workspace path that these external npm deps should be linked at. If empty, they will be linked at the root. 
<h4 id="NpmPackageInfo-sources">sources</h4>

 Depset of direct & transitive source files in these external npm package(s) and transitive dependencies 
<h4 id="NpmPackageInfo-workspace">workspace</h4>

 The workspace name that these external npm package(s) are provided from 


## declaration_info

**USAGE**

<pre>
declaration_info(<a href="#declaration_info-declarations">declarations</a>, <a href="#declaration_info-deps">deps</a>)
</pre>

Constructs a DeclarationInfo including all transitive files needed to type-check from DeclarationInfo providers in a list of deps.

**PARAMETERS**


<h4 id="declaration_info-declarations">declarations</h4>

list of typings files



<h4 id="declaration_info-deps">deps</h4>

list of labels of dependencies where we should collect their DeclarationInfo to pass transitively

Defaults to `[]`


## js_ecma_script_module_info

**USAGE**

<pre>
js_ecma_script_module_info(<a href="#js_ecma_script_module_info-sources">sources</a>, <a href="#js_ecma_script_module_info-deps">deps</a>)
</pre>

Constructs a JSEcmaScriptModuleInfo including all transitive sources from JSEcmaScriptModuleInfo providers in a list of deps.

Returns a single JSEcmaScriptModuleInfo.

**PARAMETERS**


<h4 id="js_ecma_script_module_info-sources">sources</h4>




<h4 id="js_ecma_script_module_info-deps">deps</h4>


Defaults to `[]`


## js_module_info

**USAGE**

<pre>
js_module_info(<a href="#js_module_info-sources">sources</a>, <a href="#js_module_info-deps">deps</a>)
</pre>

Constructs a JSModuleInfo including all transitive sources from JSModuleInfo providers in a list of deps.

Returns a single JSModuleInfo.

**PARAMETERS**


<h4 id="js_module_info-sources">sources</h4>




<h4 id="js_module_info-deps">deps</h4>


Defaults to `[]`


## js_named_module_info

**USAGE**

<pre>
js_named_module_info(<a href="#js_named_module_info-sources">sources</a>, <a href="#js_named_module_info-deps">deps</a>)
</pre>

Constructs a JSNamedModuleInfo including all transitive sources from JSNamedModuleInfo providers in a list of deps.

Returns a single JSNamedModuleInfo.

**PARAMETERS**


<h4 id="js_named_module_info-sources">sources</h4>




<h4 id="js_named_module_info-deps">deps</h4>


Defaults to `[]`


## run_node

**USAGE**

<pre>
run_node(<a href="#run_node-ctx">ctx</a>, <a href="#run_node-inputs">inputs</a>, <a href="#run_node-arguments">arguments</a>, <a href="#run_node-executable">executable</a>, <a href="#run_node-chdir">chdir</a>, <a href="#run_node-kwargs">kwargs</a>)
</pre>

Helper to replace ctx.actions.run

This calls node programs with a node_modules directory in place


**PARAMETERS**


<h4 id="run_node-ctx">ctx</h4>

rule context from the calling rule implementation function



<h4 id="run_node-inputs">inputs</h4>

list or depset of inputs to the action



<h4 id="run_node-arguments">arguments</h4>

list or ctx.actions.Args object containing arguments to pass to the executable



<h4 id="run_node-executable">executable</h4>

stringy representation of the executable this action will run, eg eg. "my_executable" rather than ctx.executable.my_executable



<h4 id="run_node-chdir">chdir</h4>

directory we should change to be the working dir

Defaults to `None`

<h4 id="run_node-kwargs">kwargs</h4>

all other args accepted by ctx.actions.run




## node_modules_aspect

**USAGE**

<pre>
node_modules_aspect(<a href="#node_modules_aspect-name">name</a>)
</pre>



**ASPECT ATTRIBUTES**

<h4>deps</h4>

**ATTRIBUTES**

<h4 id="node_modules_aspect-name">name</h4>

(*<a href="https://bazel.build/docs/build-ref.html#name">Name</a>, {util.mandatoryString(name: "name"
doc_string: "A unique name for this target."
type: NAME
mandatory: true
)}*)
 A unique name for this target. 


