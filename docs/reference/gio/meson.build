if get_option('gtk_doc')
  subdir('gdbus-object-manager-example')
  subdir('xml')

  ignore_headers = [
    'gdbus-2.0',
    'inotify',
    'kqueue',
    'libasyncns',
    'tests',
    'win32',
    'xdgmime',
    'gappinfoprivate.h',
    'gapplicationimpl.h',
    'gasynchelper.h',
    'gcontenttypeprivate.h',
    'gcontextspecificgroup.h',
    'gcredentialsprivate.h',
    'gdbus-daemon-generated.h',
    'gdbusactiongroup-private.h',
    'gdbusauth.h',
    'gdbusauthmechanismanon.h',
    'gdbusauthmechanismexternal.h',
    'gdbusauthmechanism.h',
    'gdbusauthmechanismsha1.h',
    'gdbusdaemon.h',
    'gdbusprivate.h',
    'gdelayedsettingsbackend.h',
    'gdocumentportal.h',
    'gdummyfile.h',
    'gdummyproxyresolver.h',
    'gdummytlsbackend.h',
    'gfileattribute-priv.h',
    'gfileinfo-priv.h',
    'ghttpproxy.h',
    'giomodule-priv.h',
    'gioprivate.h',
    'giowin32-afunix.h',
    'giowin32-priv.h',
    'gio_probes.h',
    'gio_trace.h',
    'gio-tool.h',
    'glocaldirectorymonitor.h',
    'glocalfileenumerator.h',
    'glocalfile.h',
    'glocalfileinfo.h',
    'glocalfileinputstream.h',
    'glocalfileiostream.h',
    'glocalfilemonitor.h',
    'glocalfileoutputstream.h',
    'glocalvfs.h',
    'gmemorymonitordbus.h',
    'gmemorymonitorportal.h',
    'gmountprivate.h',
    'gnativevolumemonitor.h',
    'gnetworkingprivate.h',
    'gnetworkmonitorbase.h',
    'gnetworkmonitornetlink.h',
    'gnetworkmonitornm.h',
    'gnetworkmonitorportal.h',
    'gnotificationbackend.h',
    'gnotification-private.h',
    'gopenuriportal.h',
    'gpollfilemonitor.h',
    'gportalsupport.h',
    'gpowerprofilemonitordbus.h',
    'gpowerprofilemonitorportal.h',
    'gproxyresolverportal.h',
    'gregistrysettingsbackend.h',
    'gresourcefile.h',
    'gsandbox.h',
    'gsettingsbackendinternal.h',
    'gsettings-mapping.h',
    'gsettingsschema-internal.h',
    'gsocketinputstream.h',
    'gsocketoutputstream.h',
    'gsocks4aproxy.h',
    'gsocks4proxy.h',
    'gsocks5proxy.h',
    'gsubprocesslauncher-private.h',
    'gthreadedresolver.h',
    'gtrashportal.h',
    'gunionvolumemonitor.h',
    'gunixmount.h',
    'gunixresolver.h',
    'gunixvolume.h',
    'gunixvolumemonitor.h',
    'gwin32networkmonitor.h',
    'gwin32api-application-activation-manager.h',
    'gwin32api-iterator.h',
    'gwin32api-misc.h',
    'gwin32api-package.h',
    'gwin32api-storage.h',
    'gwin32appinfo.h',
    'gwin32file-sync-stream.h',
    'gwin32mount.h',
    'gwin32packageparser.h',
    'gwin32resolver.h',
    'gwin32volumemonitor.h',
    'thumbnail-verify.h',
    'xdp-dbus.h',
    'gio-visibility.h',
  ]

  sections_files = files('gio-sections-common.txt')

  if host_system == 'windows'
    ignore_headers += [
      'gfiledescriptorbased.h',
      'gunixmounts.h',
      'gunixfdmessage.h',
      'gunixinputstream.h',
      'gunixoutputstream.h',
      'gdesktopappinfo.h',
      'gosxappinfo.h',
    ]
    sections_files += files('gio-sections-win32.txt')
    platform_file = files('gio-docs-win32.xml')
  else
    if glib_have_cocoa
      ignore_headers += ['gdesktopappinfo.h']
    else
      ignore_headers += ['gosxappinfo.h']
    endif

    ignore_headers += [
      'gwin32inputstream.h',
      'gwin32outputstream.h',
      'gwin32registrykey.h',
    ]
    platform_file = files('gio-docs-unix.xml')
  endif

  ignore_sources = [
    'kqueue',
    'tests',
    'gdbus-daemon-generated.c',
    'xdp-dbus.c',
  ]

  docpath = join_paths(glib_datadir, 'gtk-doc', 'html')
  version_conf = configuration_data()
  version_conf.set('VERSION', meson.project_version())
  configure_file(
    input: 'version.xml.in',
    output: 'version.xml',
    configuration: version_conf
  )

  concat_files_helper = find_program('concat-files-helper.py')
  configure_file(
    output : 'gio-sections.txt',
    input : sections_files,
    command : [concat_files_helper, '@OUTPUT@', '@INPUT@'],
  )

  fs.copyfile(platform_file, 'gio-docs-platform.xml')

  content_files = [
    'migrating-posix.xml',
    'gio-querymodules.xml',
    'glib-compile-schemas.xml',
    'glib-compile-resources.xml',
    'gapplication.xml',
    'gsettings.xml',
    'gresource.xml',
    'gdbus.xml',
    'gdbus-codegen.xml',
  ]

  content_files += [
    gdbus_example_objectmanager_xml,
    gdbus_example_objectmanager_sources,
    gdbus_object_manager_example_doc
  ]

  gnome.gtkdoc('gio',
    main_xml : 'gio-docs.xml',
    namespace : 'g',
    mode : 'none',
    dependencies : [libgio_dep, libgobject_dep, libglib_dep],
    src_dir : 'gio',
    scan_args : [
      '--ignore-decorators=' + ignore_decorators.replace('GLIB', 'GIO'),
      '--rebuild-types',
      '--ignore-headers=' + ' '.join(ignore_headers),
    ],
    mkdb_args : [
      '--ignore-files=' + ' '.join(ignore_sources),
    ],
    content_files : content_files,
    expand_content_files : [
      'migrating-posix.xml',
      'gdbus-codegen.xml',
    ],
    html_assets : [
      'gvfs-overview.png',
      'menu-example.png',
      'menu-model.png',
    ],
    fixxref_args: [
      '--html-dir=' + docpath,
      '--extra-dir=' + join_paths('gio', '..', 'glib', 'html'),
      '--extra-dir=' + join_paths('gio', '..', 'gobject', 'html'),
    ],
    install: true,
    check: false,
  )
endif


if get_option('man')
  manpages = ['gapplication', 'gio-querymodules', 'glib-compile-schemas',
              'glib-compile-resources', 'gsettings', 'gresource', 'gdbus',
              'gio', 'gdbus-codegen']
  foreach page : manpages
    custom_target(page + '-man',
      input: page + '.xml',
      output: page + '.1',
      command: xsltproc_command,
      install: true,
      install_dir: man1_dir)
  endforeach
endif

# gi-docgen version
expand_content_files = [
  'dbus-error.md',
  'dbus-introspection.md',
  'dbus-name-owning.md',
  'dbus-name-watching.md',
  'dbus-utils.md',
  'error.md',
  'file-attributes.md',
  'migrating-gconf.md',
  'migrating-gdbus.md',
  'migrating-gnome-vfs.md',
  'overview.md',
  'tls-overview.md',
]

gio_toml = configure_file(input: 'gio.toml.in', output: 'gio.toml', configuration: toml_conf)

custom_target('gio-docs',
  input: [ gio_toml, gio_gir[0] ],
  output: 'gio',
  command: [
    gidocgen,
    'generate',
    gidocgen_common_args,
    '--config=@INPUT0@',
    '--output-dir=@OUTPUT@',
    '--content-dir=@0@'.format(meson.current_source_dir()),
    '--add-include-path=@0@'.format(meson.current_build_dir() / '../../../gobject'),
    '--add-include-path=@0@'.format(meson.current_build_dir() / '../../../gmodule'),
    '--add-include-path=@0@'.format(meson.current_build_dir() / '../../../gio'),
    '@INPUT1@',
  ],
  build_by_default: true,
  depend_files: expand_content_files,
)
