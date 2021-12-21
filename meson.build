project('snow-extension',
  version: '41.0',
  meson_version: '>= 0.40.0'
)

sources = [
  'extension.js',
  'snow.js',
]
extra_sources = [
  'COPYING',
]
dist_files = sources + extra_sources

i18n = import('i18n')
subdir('locale')

uuid = 'snow@endlessos.org'

ver_arr = meson.project_version().split('.')
if ver_arr[1].to_int().is_even()
  shell_version = '@0@.@1@'.format(ver_arr[0], ver_arr[1])
else
  shell_version = '.'.join(ver_arr)
endif

datadir = get_option('datadir')
extensiondir = join_paths(datadir, 'gnome-shell', 'extensions')

metaconf = configuration_data()
metaconf.set('uuid', uuid)
metaconf.set('version', 2)
metaconf.set('shell_version', shell_version)

meta = [
  configure_file(
    input: 'metadata.json.in',
    output: 'metadata.json',
    configuration: metaconf
  )
]

install_data(sources + extra_sources + meta,
  install_dir: join_paths(extensiondir, uuid)
)

extension_tool = find_program('gnome-extensions', required: false)
if extension_tool.found()
  builddir = meson.current_build_dir()
  srcdir = meson.current_source_dir()

  command = [
    extension_tool,
    'pack',
    '--force',
    '--out-dir=@0@'.format(builddir),
    '--extra-source=@0@/stylesheet.css'.format(builddir),
    '--extra-source=@0@/metadata.json'.format(builddir),
  ]

  foreach s : dist_files
    command += '--extra-source=@0@'.format(s)
  endforeach

  command += srcdir

  run_target('zip-file', command: command)
endif
