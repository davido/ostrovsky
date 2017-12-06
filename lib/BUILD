java_plugin(
    name = "auto-annotation-plugin",
    processor_class = "com.google.auto.value.processor.AutoAnnotationProcessor",
    deps = [
        ":javax-annotation",
        "@auto_value//jar",
    ],
)

java_plugin(
    name = "auto-value-plugin",
    processor_class = "com.google.auto.value.processor.AutoValueProcessor",
    deps = [
        ":javax-annotation",
        "@auto_value//jar",
    ],
)

java_library(
    name = "javax-annotation",
    exports = ["@javax_annotation//jar"],
)

java_library(
    name = "auto-value",
    exported_plugins = [
        ":auto-annotation-plugin",
        ":auto-value-plugin",
    ],
    visibility = ["//visibility:public"],
    exports = ["@auto_value//jar"],
)
