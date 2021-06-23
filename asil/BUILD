package(default_visibility = ["//visibility:public"])

constraint_setting(name = "asil")

constraint_value(
    name = "qm",
    constraint_setting = ":asil",
)

constraint_value(
    name = "a",
    constraint_setting = ":asil",
)

constraint_value(
    name = "b",
    constraint_setting = ":asil",
)

constraint_value(
    name = "c",
    constraint_setting = ":asil",
)

constraint_value(
    name = "d",
    constraint_setting = ":asil",
)

# The order of the values is significant. A target constrained to ASIL B can
# depend on targets constrained to ASIL B, ASIL C, and ASIL D, but not on
# targets constrained to QM and ASIL A.
constraint_order(
    order = [
        ":qm",
        ":a",
        ":b",
        ":c",
        ":d",
    ],
)
