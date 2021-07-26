# Bazel constrain_to

This repository shows how the new attribute `constrain_to` should work. The concept is a generalization of [`testonly`](https://docs.bazel.build/versions/main/be/common-definitions.html#common.testonly):

> Because testonly is enforced at build time, not run time, and propagates virally through the dependency tree, it should be applied judiciously. For example, stubs and fakes that are useful for unit tests may also be useful for integration tests involving the same binaries that will be released to production, and therefore should probably not be marked testonly. Conversely, rules that are dangerous to even link in, perhaps because they unconditionally override normal behavior, should definitely be marked testonly.

The official feature request is https://github.com/bazelbuild/bazel/issues/13740.

## Constraints

From https://docs.bazel.build/versions/main/platforms.html:

> A constraint is a dimension in which build or production environments may differ, such as CPU architecture, the presence or absence of a GPU, or the version of a system-installed compiler. A platform is a named collection of choices for these constraints, representing the particular resources that are available in some environment. Modeling the environment as a platform helps Bazel to automatically select the appropriate toolchains for build actions.

The canonical Bazel contraints are defined in https://github.com/bazelbuild/platforms.

### constrain_to

`constrain_to` constrains a target to specific `constraint_value`s. Those constraints propagate virally through the dependency tree.

### constraint_order

`constraint_order` (see example in [asil/BUILD](asil/BUILD)) establishes a total ordering for the `constraint_value`s of a `constraint_setting`.

This is rule is useful when the `constraint_value`s aren't independent of each other, but instead relate to each other and the relationship has an inherent ordering. A good example is the [Automotive Safety Integrity Level](https://en.wikipedia.org/wiki/Automotive_Safety_Integrity_Level) (ASIL), where QM < ASIL A < ASIL B < ASIL C < ASIL D, and A < B means that A has a lower integrity than B.

## Use Cases

### Constrain Dependencies

Enforce that libraries of higher [ASIL](https://en.wikipedia.org/wiki/Automotive_Safety_Integrity_Level) (e.g., ASIL D) don't depend on libraries of lower ASIL (e.g., ASIL A) at build time.

### Verify Constraints

Examples:

1. `bazel query` all sources of ASIL B constrained `cc_*` targets and check if they are compliant with this integrity level. Example: `#include <iostream>` might not be allowed in ASIL B code.
1. `bazel query` ASIL B constrained `cc_*` targets and check if the C++ toolchain compiles them with the expected compiler and linker flags of the compiler safety manual.
