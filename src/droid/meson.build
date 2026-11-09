droid_sink = library('droid-sink',
  ['droid-sink.c', 'droid-sink.h'],
  dependencies : libdroid_util_dep,
  pic : true,
  install : true,
  install_dir : modlibexecdir,
  install_rpath : rpath_dirs,
)

droid_sink_dep = declare_dependency(
  link_with : droid_sink,
  compile_args : pa_c_args + ['-DPULSEAUDIO_VERSION=@0@'.format(pa_version_major)],
  dependencies : libdroid_util_dep,
  include_directories : [configinc],
)

droid_source = library('droid-source',
  ['droid-source.c', 'droid-source.h'],
  dependencies : libdroid_util_dep,
  pic : true,
  install : true,
  install_dir : modlibexecdir,
  install_rpath : rpath_dirs,
)

droid_source_dep = declare_dependency(
  link_with : droid_source,
  compile_args : pa_c_args + ['-DPULSEAUDIO_VERSION=@0@'.format(pa_version_major)],
  dependencies : libdroid_util_dep,
  include_directories : [configinc],
)

module_sink = shared_module('module-droid-sink',
  'module-droid-sink.c',
  name_prefix : '',
  c_args : '-DPA_MODULE_NAME=module_droid_sink',
  dependencies : [droid_sink_dep, libdroid_util_dep],
  install : true,
  install_dir : modlibexecdir,
  install_rpath : rpath_dirs,
)

module_sink = shared_module('module-droid-source',
  'module-droid-source.c',
  name_prefix : '',
  c_args : '-DPA_MODULE_NAME=module_droid_source',
  dependencies : [droid_source_dep, libdroid_util_dep],
  install : true,
  install_dir : modlibexecdir,
  install_rpath : rpath_dirs,
)

module_sink = shared_module('module-droid-card',
  ['module-droid-card.c', 'droid-extcon.c', 'droid-extevdev.c'],
  name_prefix : '',
  c_args : '-DPA_MODULE_NAME=module_droid_card',
  dependencies : [droid_sink_dep, droid_source_dep, libdroid_util_dep, udev_dep, evdev_dep],
  install : true,
  install_dir : modlibexecdir,
  install_rpath : rpath_dirs,
)

# PulseAudio loads a module only if it declares PulseAudio's own version, so
# module-droid-fm declares major.minor of the libpulse it is built against.
fm_pulse_version = dependency('libpulse', version : '>= 14.2', required : true).version().split('-')[0].split('.')

module_fm = shared_module('module-droid-fm',
  'module-droid-fm.c',
  name_prefix : '',
  c_args : ['-DPA_MODULE_NAME=module_droid_fm',
            '-DPULSEAUDIO_PACKAGE_VERSION="@0@.@1@"'.format(fm_pulse_version[0], fm_pulse_version[1])],
  dependencies : [libdroid_util_dep],
  install : true,
  install_dir : modlibexecdir,
  install_rpath : rpath_dirs,
)
