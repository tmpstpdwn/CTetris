project(
  'cTetris',
  'c',
  version: '1.0.1',
  default_options: [
    'warning_level=3',
    'c_std=c99',
  ],
)

cc = meson.get_compiler('c')
system = host_machine.system()

if cc.get_id() == 'emscripten'
  add_project_arguments('-std=gnu99', language: 'c',)
endif

cmake = import('cmake')

raylib_options = cmake.subproject_options()

raylib_options.add_cmake_defines({
  'BUILD_EXAMPLES': 'OFF',
})

if system == 'linux'
  raylib_options.add_cmake_defines({
    'GLFW_BUILD_WAYLAND': 'ON',
  })
elif system == 'emscripten'
  raylib_options.add_cmake_defines({
    'PLATFORM': 'Web',
  })
endif

raylib_proj = cmake.subproject(
  'raylib',
  options: raylib_options,
)

raylib = raylib_proj.dependency('raylib')

sources = files(
  'src/main.c',
  'src/renderer.c',
  'src/assets.c',
  'src/score.c',
  'src/ctetris.c',
)

win_subsystem = 'console'

if system == 'windows'
  windows = import('windows')
  sources += windows.compile_resources(
    'platform/windows/resource.rc',
  )
  if get_option('buildtype') == 'release'
    win_subsystem = 'windows'

    add_project_link_arguments(
      '/ENTRY:mainCRTStartup',
      language: 'c',
    )
  endif
endif

executable(
  'cTetris',
  sources,
  include_directories: include_directories('include'),
  dependencies: raylib,
  win_subsystem: win_subsystem,
)
