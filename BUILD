licenses(["notice"])

exports_files([
  "LICENSE",
  "NOTICE",
])

COPTS = [
  "-Wall",
  "-Wextra",
  "-pedantic",
  "-Wfatal-errors",
  "-std=c++11",
  "-march=native",
  "-g",
  "-O3",
  "-flto",
]

LIBS = [
  "@libprim//:prim",
  "@libstrop//:strop",
  "@libfio//:fio",
  "@libsettings//:settings",
  "@jsoncpp//:jsoncpp",
]

cc_library(
  name = "lib",
  srcs = glob(
    ["src/**/*.cc"],
    exclude = [
      "src/main.cc",
      "src/**/*_TEST*",
    ],
  ),
  hdrs = glob(
    [
      "src/**/*.h",
      "src/**/*.tcc",
    ],
    exclude = ["src/**/*_TEST*"],
  ),
  copts = COPTS,
  deps = LIBS,
  includes = [
    "src",
  ],
  visibility = ["//visibility:private"],
  alwayslink = 1,
)

cc_binary(
  name = "settingstest",
  srcs = ["src/main.cc"],
  includes = [
    "src",
  ],
  copts = COPTS,
  deps = [
    ":lib",
  ] + LIBS,
  visibility = ["//visibility:public"],
)

cc_library(
  name = "test_lib",
  testonly = 1,
  srcs = glob([
    "src/**/*_TEST*.cc",
  ]),
  hdrs = glob([
    "src/**/*_TEST*.h",
    "src/**/*_TEST*.tcc",
  ]),
  copts = COPTS,
  deps = [
    ":lib",
    "@googletest//:gtest_main",
  ] + LIBS,
  visibility = ["//visibility:private"],
  alwayslink = 1,
)

cc_test(
  name = "settingstest_test",
  copts = COPTS,
  deps = [
    ":test_lib",
  ] + LIBS,
  args = [
    "--gtest_color=yes",
  ],
  visibility = ["//visibility:public"],
)

genrule(
  name = "lint",
  srcs = glob([
    "src/**/*.cc"
  ]) + glob([
    "src/**/*.h",
    "src/**/*.tcc",
  ]),
  outs = ["linted"],
  cmd = """
    python $(location @cpplint//:cpplint) \
      --root=$$(pwd)/src \
      --headers=h,tcc \
      --extensions=cc,h,tcc \
      --quiet $(SRCS) > $@
    echo // $$(date) > $@
  """,
  tools = [
    "@cpplint//:cpplint",
  ],
  visibility = ["//visibility:public"],
)
