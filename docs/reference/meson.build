# The list of minor versions in the 2.x.x series which have had
# GLIB_AVAILABLE_IN_* macros. This should include the current unreleased stable
# version.
first_version = 26
last_version = minor_version.is_odd() ? minor_version + 1 : minor_version

ignore_decorators = [
  'GLIB_VAR',
  'G_GNUC_INTERNAL',
  'G_GNUC_WARN_UNUSED_RESULT',
  'GLIB_AVAILABLE_IN_ALL',
]

foreach i : range(first_version, last_version + 2, 2)
  version = i.to_string()
  ignore_decorators += [
    # Note that gtkdoc is going to use those in regex, and the longest match
    # must come first. That's why '_FOR()' variant comes first.
    # gtkdoc special-case '()' and replace it by a regex matching a symbol name.
    'GLIB_AVAILABLE_IN_2_' + version,
    'GLIB_DEPRECATED_IN_2_' + version + '_FOR()',
    'GLIB_DEPRECATED_IN_2_' + version,

    'GLIB_AVAILABLE_STATIC_INLINE_IN_2_' + version,

    'GLIB_AVAILABLE_ENUMERATOR_IN_2_' + version,
    'GLIB_DEPRECATED_ENUMERATOR_IN_2_' + version + '_FOR()',
    'GLIB_DEPRECATED_ENUMERATOR_IN_2_' + version,

    'GLIB_AVAILABLE_MACRO_IN_2_' + version,
    'GLIB_DEPRECATED_MACRO_IN_2_' + version + '_FOR()',
    'GLIB_DEPRECATED_MACRO_IN_2_' + version,

    'GLIB_AVAILABLE_TYPE_IN_2_' + version,
    'GLIB_DEPRECATED_TYPE_IN_2_' + version + '_FOR()',
    'GLIB_DEPRECATED_TYPE_IN_2_' + version,
  ]
endforeach

ignore_decorators = '|'.join(ignore_decorators)

if get_option('gtk_doc')
  # Check we have the minimum gtk-doc version required. Older versions won't
  # generate correct documentation.
  dependency('gtk-doc', version : '>=1.32.1',
    fallback : ['gtk-doc', 'dummy_dep'],
    default_options : ['tests=false'])

  # We cannot built the API reference off of a static library,
  # as symbols might get dropped by the linker
  if not glib_build_shared
    error('The API reference can only be built against a shared library')
  endif
endif

# gi-docgen version
if get_option('gtk_doc') and enable_gir
  gidocgen_dep = dependency('gi-docgen', version: '>= 2023.1',
                          fallback: ['gi-docgen', 'dummy_dep'],
                          required: true)

  toml_conf = configuration_data()
  toml_conf.set('VERSION', meson.project_version())

  gidocgen = find_program('gi-docgen', required: true)

  gidocgen_common_args = [
    '--quiet',
    '--no-namespace-dir',
    '--fatal-warnings',
  ]

  subdir('glib')
  subdir('gmodule')
  subdir('gobject')
  subdir('gio')
endif
