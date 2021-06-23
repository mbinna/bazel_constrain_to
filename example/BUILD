# QM library can depend on all libraries, because it is unconstrained.
cc_library(
    name = "qm",
    srcs = ["qm.cc"],
    hdrs = ["qm.h"],
    deps = [
        ":b",
        ":d",
    ],
)

# ASIL B library con only depend on ASIL B, ASIL C, and ASIL D libraries.
cc_library(
    name = "b",
    srcs = ["b.cc"],
    hdrs = ["b.h"],
    constrain_to = ["//asil:b"],
    deps = [":d"],
)

cc_library(
    name = "d",
    srcs = ["d.cc"],
    hdrs = ["d.h"],
    constrain_to = ["//asil:d"],
)

# QM binary can depend on all libraries, because it is unconstrained.
cc_binary(
    name = "qm_app",
    srcs = ["qm_app.cc"],
    deps = [
        ":b",
        ":d",
        ":qm",
    ],
)

# ASIL B binary can only depend on ASIL B, ASIL C, and ASIL D libraries.
cc_binary(
    name = "b_app",
    srcs = ["b_app.cc"],
    constrain_to = ["//asil:b"],
    deps = [
        ":b",
        ":d",
    ],
)

# ASIL D binary can only depend on ASIL D libraries.
cc_library(
    name = "d_app",
    srcs = ["d_app.cc"],
    constrain_to = ["//asil:d"],
)
