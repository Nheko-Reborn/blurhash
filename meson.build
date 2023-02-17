project('blurhash', 'cpp',
  version : '0.2.0',
  default_options : ['warning_level=3', 'cpp_std=c++20'])

lib = library('blurhash',
           'blurhash.cpp',
           include_directories: '.',
           version: meson.project_version(),
           install : true)

if get_option('tests')
  doctest_dep = dependency('doctest', required: true)

  tests = executable('blurhash-tests',
    'blurhash.cpp',
    cpp_args : '-DDOCTEST_CONFIG_IMPLEMENT_WITH_MAIN',
    dependencies: doctest_dep,
    install : false)
  test('blurhash-tests', tests)
endif

if get_option('benchmarks')
  bench = executable('blurhash-decode-bench', 'blurhash-decode-bench.cpp', link_with: lib, install: false)
  benchmark('blurhash-decode-bench', bench, timeout: 60, should_fail: true)
endif

if get_option('examples')
  executable('blurhash2bmp', 'blurhash2bmp.cpp', link_with: lib, install: true)
  executable('blurhash', 'image2blurhash.cpp', link_with: lib, install: true)
endif

install_headers('blurhash.hpp')

pkg = import('pkgconfig')
pkg.generate(libraries : [lib],
             subdirs : ['.'],
             version : meson.project_version(),
             name : meson.project_name(),
             filebase : meson.project_name(),
             description : 'C++ blurhash encoder/decoder')

blurhash_dep = declare_dependency(link_with: lib, include_directories: '.')
meson.override_dependency('blurhash', blurhash_dep)
