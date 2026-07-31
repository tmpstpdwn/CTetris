project(
  'cTetris',
  'c',
  version: '1.0.1',
  default_options: [
    'warning_level=3'
  ]
)

cc = meson.get_compiler('c')

if cc.get_id() == 'emscripten'
  add_project_arguments('-std=gnu99', language: 'c')
else
  add_project_arguments('-std=c99', language: 'c')
endif

if get_option('sanitize')
  add_project_arguments(
    '-fsanitize=address,undefined',
    '-fno-omit-frame-pointer',
    '-g',
    language: 'c'
  )

  add_project_link_arguments(
    '-fsanitize=address,undefined',
    language: 'c'
  )
endif

cmake = import('cmake')

raylib_options = cmake.subproject_options()

raylib_options.add_cmake_defines({
  'BUILD_EXAMPLES': 'OFF',
})

if host_machine.system() == 'linux'
  raylib_options.add_cmake_defines({
    'GLFW_BUILD_WAYLAND': 'ON',
  })
endif

if cc.get_id() == 'emscripten'
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

if host_machine.system() == 'windows'
  sources += files('platform/windows/resource.rc')
endif

win_subsystem = 'console'

if host_machine.system() == 'windows'
  if get_option('buildtype') == 'release'
    win_subsystem = 'windows'
  endif
endif

executable(
  'cTetris',
  sources,
  include_directories: include_directories('include'),
  dependencies: raylib,
  win_subsystem: win_subsystem,
)
