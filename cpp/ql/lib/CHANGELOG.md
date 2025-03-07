## 0.12.2

No user-facing changes.

## 0.12.1

### New Features

* Added an `isPrototyped` predicate to `Function` that holds when the function has a prototype.

## 0.12.0

### Breaking Changes

* The expressions `AssignPointerAddExpr` and `AssignPointerSubExpr` are no longer subtypes of `AssignBitwiseOperation`.

### Minor Analysis Improvements

* The "Returning stack-allocated memory" (`cpp/return-stack-allocated-memory`) query now also detects returning stack-allocated memory allocated by calls to `alloca`, `strdupa`, and `strndupa`.
* Added models for `strlcpy` and `strlcat`.
* Added models for the `sprintf` variants from the `StrSafe.h` header.
* Added SQL API models for `ODBC`.
* Added taint models for `realloc` and related functions.

## 0.11.0

### Breaking Changes

* The `Container` and `Folder` classes now derive from `ElementBase` instead of `Locatable`, and no longer expose the `getLocation` predicate. Use `getURL` instead.

### New Features

* Added a new class `AdditionalCallTarget` for specifying additional call targets.

### Minor Analysis Improvements

* More field accesses are identified as `ImplicitThisFieldAccess`.
* Added support for new floating-point types in C23 and C++23.

## 0.10.1

### Minor Analysis Improvements

* Deleted the deprecated `AnalysedString` class, use the new name `AnalyzedString`.
* Deleted the deprecated `isBarrierGuard` predicate from the dataflow library and its uses, use `isBarrier` and the `BarrierGuard` module instead.

## 0.10.0

### Minor Analysis Improvements

* Functions that do not return due to calling functions that don't return (e.g. `exit`) are now detected as
 non-returning in the IR and dataflow.
* Treat functions that reach the end of the function as returning in the IR.
  They used to be treated as unreachable but it is allowed in C. 
* The `DataFlow::asDefiningArgument` predicate now takes its argument from the range starting at `1` instead of `2`. Queries that depend on the single-parameter version of `DataFlow::asDefiningArgument` should have their arguments updated accordingly.

## 0.9.3

No user-facing changes.

## 0.9.2

### Deprecated APIs

* `getAllocatorCall` on `DeleteExpr` and `DeleteArrayExpr` has been deprecated. `getDeallocatorCall` should be used instead.

### New Features

* Added `DeleteOrDeleteArrayExpr` as a super type of `DeleteExpr` and `DeleteArrayExpr`

### Minor Analysis Improvements

* `delete` and `delete[]` are now modeled as calls to the relevant `operator delete` in the IR. In the case of a dynamic delete call a new instruction `VirtualDeleteFunctionAddress` is used to represent a function that dispatches to the correct delete implementation.
* Only the 2 level indirection of `argv` (corresponding to `**argv`) is consided for `FlowSource`.

## 0.9.1

No user-facing changes.

## 0.9.0

### Breaking Changes

* The `shouldPrintFunction` predicate from `PrintAstConfiguration` has been replaced by `shouldPrintDeclaration`. Users should now override `shouldPrintDeclaration` if they want to limit the declarations that should be printed.
* The `shouldPrintFunction` predicate from `PrintIRConfiguration` has been replaced by `shouldPrintDeclaration`. Users should now override `shouldPrintDeclaration` if they want to limit the declarations that should be printed.

### Major Analysis Improvements

* The `PrintAST` library now also prints global and namespace variables and their initializers.

### Minor Analysis Improvements

* The `_Float128x` type is no longer exposed as a builtin type. As this type could not occur any code base, this should only affect queries that explicitly looked at the builtin types.

## 0.8.1

### Deprecated APIs

* The library `semmle.code.cpp.dataflow.DataFlow` has been deprecated. Please use `semmle.code.cpp.dataflow.new.DataFlow` instead.

### New Features

* The `DataFlow::StateConfigSig` signature module has gained default implementations for `isBarrier/2` and `isAdditionalFlowStep/4`. 
  Hence it is no longer needed to provide `none()` implementations of these predicates if they are not needed.

### Minor Analysis Improvements

* Data flow configurations can now include a predicate `neverSkip(Node node)`
  in order to ensure inclusion of certain nodes in the path explanations. The
  predicate defaults to the end-points of the additional flow steps provided in
  the configuration, which means that such steps now always are visible by
  default in path explanations.
* The `IRGuards` library has improved handling of pointer addition and subtraction operations.

## 0.8.0

### New Features

* The `ProductFlow::StateConfigSig` signature now includes default predicates for `isBarrier1`, `isBarrier2`, `isAdditionalFlowStep1`, and `isAdditionalFlowStep1`. Hence, it is no longer needed to provide `none()` implementations of these predicates if they are not needed.

### Minor Analysis Improvements

* Deleted the deprecated `getURL` predicate from the `Container`, `Folder`, and `File` classes. Use the `getLocation` predicate instead.

## 0.7.4

No user-facing changes.

## 0.7.3

### Minor Analysis Improvements

* Deleted the deprecated `hasCopyConstructor` predicate from the `Class` class in `Class.qll`.
* Deleted many deprecated predicates and classes with uppercase `AST`, `SSA`, `CFG`, `API`, etc. in their names. Use the PascalCased versions instead.
* Deleted the deprecated `CodeDuplication.qll` file.

## 0.7.2

### New Features

* Added an AST-based interface (`semmle.code.cpp.rangeanalysis.new.RangeAnalysis`) for the relative range analysis library.
* A new predicate `BarrierGuard::getAnIndirectBarrierNode` has been added to the new dataflow library (`semmle.code.cpp.dataflow.new.DataFlow`) to mark indirect expressions as barrier nodes using the `BarrierGuard` API.

### Major Analysis Improvements

* In the intermediate representation, handling of control flow after non-returning calls has been improved. This should remove false positives in queries that use the intermedite representation or libraries based on it, including the new data flow library.

### Minor Analysis Improvements

* The `StdNamespace` class now also includes all inline namespaces that are children of `std` namespace.
* The new dataflow (`semmle.code.cpp.dataflow.new.DataFlow`) and taint-tracking libraries (`semmle.code.cpp.dataflow.new.TaintTracking`) now support tracking flow through static local variables.

## 0.7.1

No user-facing changes.

## 0.7.0

### Breaking Changes

* The internal `SsaConsistency` module has been moved from `SSAConstruction` to `SSAConsitency`, and the deprecated `SSAConsistency` module has been removed.

### Deprecated APIs

* The single-parameter predicates `ArrayOrVectorAggregateLiteral.getElementExpr` and `ClassAggregateLiteral.getFieldExpr` have been deprecated in favor of `ArrayOrVectorAggregateLiteral.getAnElementExpr` and `ClassAggregateLiteral.getAFieldExpr`.
* The recently introduced new data flow and taint tracking APIs have had a
  number of module and predicate renamings. The old APIs remain in place for
  now.
* The `SslContextCallAbstractConfig`, `SslContextCallConfig`, `SslContextCallBannedProtocolConfig`, `SslContextCallTls12ProtocolConfig`, `SslContextCallTls13ProtocolConfig`, `SslContextCallTlsProtocolConfig`, `SslContextFlowsToSetOptionConfig`, `SslOptionConfig` dataflow configurations from `BoostorgAsio` have been deprecated. Please use `SslContextCallConfigSig`, `SslContextCallGlobal`, `SslContextCallFlow`, `SslContextCallBannedProtocolFlow`, `SslContextCallTls12ProtocolFlow`, `SslContextCallTls13ProtocolFlow`, `SslContextCallTlsProtocolFlow`, `SslContextFlowsToSetOptionFlow`.

### New Features

* Added overridable predicates `getSizeExpr` and `getSizeMult` to the `BufferAccess` class (`semmle.code.cpp.security.BufferAccess.qll`). This makes it possible to model a larger class of buffer reads and writes using the library.

### Minor Analysis Improvements

* The `BufferAccess` library (`semmle.code.cpp.security.BufferAccess`) no longer matches buffer accesses inside unevaluated contexts (such as inside `sizeof` or `decltype` expressions). As a result, queries using this library may see fewer false positives.

### Bug Fixes

* Fixed some accidental predicate visibility in the backwards-compatible wrapper for data flow configurations. In particular `DataFlow::hasFlowPath`, `DataFlow::hasFlow`, `DataFlow::hasFlowTo`, and `DataFlow::hasFlowToExpr` were accidentally exposed in a single version.

## 0.6.1

No user-facing changes.

## 0.6.0

### Breaking Changes

* The `semmle.code.cpp.commons.Buffer` and `semmle.code.cpp.commons.NullTermination` libraries no longer expose `semmle.code.cpp.dataflow.DataFlow`. Please import `semmle.code.cpp.dataflow.DataFlow` directly.

### Deprecated APIs

* The `WriteConfig` taint tracking configuration has been deprecated. Please use `WriteFlow`.

### New Features

* Added support for merging two `PathGraph`s via disjoint union to allow results from multiple data flow computations in a single `path-problem` query.

### Major Analysis Improvements

* A new C/C++ dataflow library (`semmle.code.cpp.dataflow.new.DataFlow`) has been added.
  The new library behaves much more like the dataflow library of other CodeQL supported
  languages by following use-use dataflow paths instead of def-use dataflow paths.
  The new library also better supports dataflow through indirections, and new predicates
  such as `Node::asIndirectExpr` have been added to facilitate working with indirections.

  The `semmle.code.cpp.ir.dataflow.DataFlow` library is now identical to the new
  `semmle.code.cpp.dataflow.new.DataFlow` library.
* The main data flow and taint tracking APIs have been changed. The old APIs
  remain in place for now and translate to the new through a
  backwards-compatible wrapper. If multiple configurations are in scope
  simultaneously, then this may affect results slightly. The new API is quite
  similar to the old, but makes use of a configuration module instead of a
  configuration class.

### Minor Analysis Improvements

* Deleted the deprecated `hasGeneratedCopyConstructor` and `hasGeneratedCopyAssignmentOperator` predicates from the `Folder` class.
* Deleted the deprecated `getPath` and `getFolder` predicates from the `XmlFile` class.
* Deleted the deprecated `getMustlockFunction`, `getTrylockFunction`, `getLockFunction`, and `getUnlockFunction` predicates from the `MutexType` class.
* Deleted the deprecated `getPosInBasicBlock` predicate from the `SubBasicBlock` class.
* Deleted the deprecated `getExpr` predicate from the `PointerDereferenceExpr` class.
* Deleted the deprecated `getUseInstruction` and `getDefinitionInstruction` predicates from the `Operand` class.
* Deleted the deprecated `isInParameter`, `isInParameterPointer`, and `isInQualifier` predicates from the `FunctionInput` class.
* Deleted the deprecated `isOutParameterPointer`, `isOutQualifier`, `isOutReturnValue`, and `isOutReturnPointer` predicate from the `FunctionOutput` class.
* Deleted the deprecated 3-argument `isGuardPhi` predicate from the `RangeSsaDefinition` class.

## 0.5.4

No user-facing changes.

## 0.5.3

No user-facing changes.

## 0.5.2

No user-facing changes.

## 0.5.1

No user-facing changes.

## 0.5.0

### Breaking Changes

The predicates in the `MustFlow::Configuration` class used by the `MustFlow` library (`semmle.code.cpp.ir.dataflow.MustFlow`) have changed to be defined directly in terms of the C++ IR instead of IR dataflow nodes.

### Deprecated APIs

* Deprecated `semmle.code.cpp.ir.dataflow.DefaultTaintTracking`. Use `semmle.code.cpp.ir.dataflow.TaintTracking`.
* Deprecated `semmle.code.cpp.security.TaintTrackingImpl`. Use `semmle.code.cpp.ir.dataflow.TaintTracking`.
* Deprecated `semmle.code.cpp.valuenumbering.GlobalValueNumberingImpl`. Use `semmle.code.cpp.valuenumbering.GlobalValueNumbering`, which exposes the same API.

### Minor Analysis Improvements

* The `ArgvSource` flow source now uses the second parameter of `main` as its source instead of the uses of this parameter.
* The `ArgvSource` flow source has been generalized to handle cases where the argument vector of `main` is not named `argv`.
* The `getaddrinfo` function is now recognized as a flow source.
* The `secure_getenv` and `_wgetenv` functions are now recognized as local flow sources.
* The `scanf` and `fscanf` functions and their variants are now recognized as flow sources.
* Deleted the deprecated `getName` and `getShortName` predicates from the `Folder` class.

## 0.4.6

No user-facing changes.

## 0.4.5

No user-facing changes.

## 0.4.4

No user-facing changes.

## 0.4.3

### Minor Analysis Improvements

* Fixed bugs in the `FormatLiteral` class that were causing `getMaxConvertedLength` and related predicates to return no results when the format literal was `%e`, `%f` or `%g` and an explicit precision was specified.

## 0.4.2

No user-facing changes.

## 0.4.1

No user-facing changes.

## 0.4.0

### Deprecated APIs

* Some classes/modules with upper-case acronyms in their name have been renamed to follow our style-guide. 
  The old name still exists as a deprecated alias.

### New Features

* Added subclasses of `BuiltInOperations` for `__is_same`, `__is_function`, `__is_layout_compatible`, `__is_pointer_interconvertible_base_of`, `__is_array`, `__array_rank`, `__array_extent`, `__is_arithmetic`, `__is_complete_type`, `__is_compound`, `__is_const`, `__is_floating_point`, `__is_fundamental`, `__is_integral`, `__is_lvalue_reference`, `__is_member_function_pointer`, `__is_member_object_pointer`, `__is_member_pointer`, `__is_object`, `__is_pointer`, `__is_reference`, `__is_rvalue_reference`, `__is_scalar`, `__is_signed`, `__is_unsigned`, `__is_void`, and `__is_volatile`.

### Bug Fixes

* Fixed an issue in the taint tracking analysis where implicit reads were not allowed by default in sinks or additional taint steps that used flow states.

## 0.3.5

## 0.3.4

### Deprecated APIs

* Many classes/predicates/modules with upper-case acronyms in their name have been renamed to follow our style-guide. 
  The old name still exists as a deprecated alias.

### New Features

* Added support for getting the link targets of global and namespace variables.
* Added a `BlockAssignExpr` class, which models a `memcpy`-like operation used in compiler generated copy/move constructors and assignment operations.

### Minor Analysis Improvements

* All deprecated predicates/classes/modules that have been deprecated for over a year have been deleted.

## 0.3.3

### New Features

* Added a predicate `getValueConstant` to `AttributeArgument` that yields the argument value as an `Expr` when the value is a constant expression.
* A new class predicate `MustFlowConfiguration::allowInterproceduralFlow` has been added to the `semmle.code.cpp.ir.dataflow.MustFlow` library. The new predicate can be overridden to disable interprocedural flow.
* Added subclasses of `BuiltInOperations` for `__builtin_bit_cast`, `__builtin_shuffle`, `__has_unique_object_representations`, `__is_aggregate`, and `__is_assignable`.

### Major Analysis Improvements

* The IR dataflow library now includes flow through global variables. This enables new findings in many scenarios.

## 0.3.2

### Bug Fixes

* Under certain circumstances a variable declaration that is not also a definition could be associated with a `Variable` that did not have the definition as a `VariableDeclarationEntry`. This is now fixed, and a unique `Variable` will exist that has both the declaration and the definition as a `VariableDeclarationEntry`.

## 0.3.1

### Minor Analysis Improvements

* `AnalysedExpr::isNullCheck` and `AnalysedExpr::isValidCheck` have been updated to handle variable accesses on the left-hand side of the C++ logical "and", and variable declarations in conditions.

## 0.3.0

### Deprecated APIs

* The `BarrierGuard` class has been deprecated. Such barriers and sanitizers can now instead be created using the new `BarrierGuard` parameterized module.

### Bug Fixes

* `UserType.getADeclarationEntry()` now yields all forward declarations when the user type is a `class`, `struct`, or `union`.

## 0.2.3

### New Features

* An `isBraced` predicate was added to the `Initializer` class which holds when a C++ braced initializer was used in the initialization.

## 0.2.2

### Deprecated APIs

 * The `AnalysedString` class in the `StringAnalysis` module has been replaced with `AnalyzedString`, to follow our style guide. The old name still exists as a deprecated alias.

### New Features

* A `getInitialization` predicate was added to the `ConstexprIfStmt`, `IfStmt`, and `SwitchStmt` classes that yields the C++17-style initializer of the `if` or `switch` statement when it exists.

## 0.2.1

## 0.2.0

### Breaking Changes

* The signature of `allowImplicitRead` on `DataFlow::Configuration` and `TaintTracking::Configuration` has changed from `allowImplicitRead(DataFlow::Node node, DataFlow::Content c)` to `allowImplicitRead(DataFlow::Node node, DataFlow::ContentSet c)`.

### Minor Analysis Improvements

* More Windows pool allocation functions are now detected as `AllocationFunction`s.
* The `semmle.code.cpp.commons.Buffer` library has been enhanced to handle array members of classes that do not specify a size.

## 0.1.0

### Breaking Changes

* The recently added flow-state versions of `isBarrierIn`, `isBarrierOut`, `isSanitizerIn`, and `isSanitizerOut` in the data flow and taint tracking libraries have been removed.

### New Features

* A new library `semmle.code.cpp.security.PrivateData` has been added. The new library heuristically detects variables and functions dealing with sensitive private data, such as e-mail addresses and credit card numbers.

### Minor Analysis Improvements

* The `semmle.code.cpp.security.SensitiveExprs` library has been enhanced with some additional rules for detecting credentials.

## 0.0.13

## 0.0.12

### Breaking Changes

* The flow state variants of `isBarrier` and `isAdditionalFlowStep` are no longer exposed in the taint tracking library. The `isSanitizer` and `isAdditionalTaintStep` predicates should be used instead.

### Deprecated APIs

* Many classes/predicates/modules that had upper-case acronyms have been renamed to follow our style-guide. 
  The old name still exists as a deprecated alias.

### New Features

* The data flow and taint tracking libraries have been extended with versions of `isBarrierIn`, `isBarrierOut`, and `isBarrierGuard`, respectively `isSanitizerIn`, `isSanitizerOut`, and `isSanitizerGuard`, that support flow states.

### Minor Analysis Improvements

* `DefaultOptions::exits` now holds for C11 functions with the `_Noreturn` or `noreturn` specifier.
* `hasImplicitCopyConstructor` and `hasImplicitCopyAssignmentOperator` now correctly handle implicitly-deleted operators in templates.
* All deprecated predicates/classes/modules that have been deprecated for over a year have been deleted.

## 0.0.11

### Minor Analysis Improvements

* Many queries now support structured bindings, as structured bindings are now handled in the IR translation.

## 0.0.10

### New Features

* Added a `isStructuredBinding` predicate to the `Variable` class which holds when the variable is declared as part of a structured binding declaration.

## 0.0.9


## 0.0.8

### Deprecated APIs

* The `codeql/cpp-upgrades` CodeQL pack has been removed. All upgrades scripts have been merged into the `codeql/cpp-all` CodeQL pack.

### Minor Analysis Improvements

* `FormatLiteral::getMaxConvertedLength` now uses range analysis to provide a
  more accurate length for integers formatted with `%x`

## 0.0.7

## 0.0.6

## 0.0.5

## 0.0.4

### New Features

* The QL library `semmle.code.cpp.commons.Exclusions` now contains a predicate
  `isFromSystemMacroDefinition` for identifying code that originates from a
  macro outside the project being analyzed.
