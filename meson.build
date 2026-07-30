project(
  'cTetris',
  'c',
  version: '1.0.1',
  default_options: [
    'warning_level=3'
  ]
)

if meson.get_compiler('c').get_id() == 'emscripten'
  add_project_arguments('-std=gnu99', language: 'c')
else
  add_project_arguments('-std=c99', language: 'c')
endif

cmake = import('cmake')

raylib_options = cmake.subproject_options()

raylib_options.add_cmake_defines({
  'BUILD_EXAMPLES': 'OFF',
})

if meson.get_compiler('c').get_id() == 'emscripten'
  raylib_options.add_cmake_defines({
    'PLATFORM': 'Web',
  })
endif

raylib_proj = cmake.subproject(
  'raylib',
  options: raylib_options
)

raylib = raylib_proj.dependency('raylib')

sources = files(
  'src/main.c',
  'src/renderer.c',
  'src/assets.c',
  'src/score.c',
  'src/ctetris.c',
)

executable(
  'cTetris',
  sources,
  include_directories: include_directories('include'),
  dependencies: raylib
)
