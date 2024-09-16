project('purple-facebook', 'c')

# Define package version
#PACKAGE_VERSION = '0.9.61'  # Adjust the version number as needed
#PACKAGE_URL = 'https://github.com/DMJC/purple-facebook'

# Configure the package
#pkgconf = configuration_data()
#pkgconf.set('PACKAGE_VERSION', PACKAGE_VERSION)
#pkgconf.set('PACKAGE_URL', PACKAGE_URL)

compiler_options = []
compiler_options += ['-DPACKAGE_VERSION="0.9.7"']
compiler_options += ['-DPACKAGE_URL="https://github.com/dequis/purple-facebook/"']

#define('PACKAGE_VERSION', '"0.9.61"')
#define('PACKAGE_URL', '"https://github.com/DMJC/purple-facebook"')

# Write the configuration to a file
#configure_file(output : 'config.h',
#               configuration : pkgconf)

# Add compiler flags if warnings are enabled
if get_option('warnings')
    add_project_arguments('-Wall', '-Wextra', '-Waggregate-return', '-Wdeclaration-after-statement', '-Wfloat-equal', '-Wformat', '-Winit-self', '-Wmissing-declarations', '-Wmissing-prototypes', '-Wno-unused-parameter', '-Wpointer-arith', language : 'c')
endif

# Argument for specifying the libpurple plugin directory
plugin_dir = get_option('plugindir')

plugin_cflags = ['-I@0@/include'.format(meson.current_source_dir()),
                 '-I@0@/pidgin'.format(meson.current_source_dir()),
                 '-I@0@/pidgin/libpurple'.format(meson.current_source_dir()),
                 '-Ipidgin/libpurple/',  # Adjust this path
                 '-DPURPLE_PLUGINS',
                 '-include', 'purple-compat.h']
add_project_arguments(plugin_cflags, language : 'c')

# Dependency checks using pkg-config
glib_dep = dependency('glib-2.0', version : '>=2.28.0')
json_dep = dependency('json-glib-1.0', version : '>=0.14.0')
purple_dep = dependency('purple', version : '<3')
zlib_dep = dependency('zlib')

# Check for glib-genmarshal tool
glib_genmarshal = find_program('glib-genmarshal')
if not glib_genmarshal.found()
    error('The `glib-genmarshal` tool is missing.')
endif

generate_marshal_header_command = ['glib-genmarshal', 'pidgin/libpurple/protocols/facebook/marshaller.list', '--prefix=fb_marshal', '--header', '--output', 'pidgin/libpurple/protocols/facebook/marshal.h']
generate_marshal_c_command = ['glib-genmarshal', 'pidgin/libpurple/protocols/facebook/marshaller.list', '--body', '--prefix=fb_marshal', '--output', 'pidgin/libpurple/protocols/facebook/marshal.c']
run_command(generate_marshal_header_command,
           check: true)

run_command(generate_marshal_c_command,
           check: true)

# Define source files
source_files = files(
    'pidgin/libpurple/protocols/facebook/marshal.c',
    'pidgin/libpurple/protocols/facebook/api.c',
    'pidgin/libpurple/protocols/facebook/data.c',
    'pidgin/libpurple/protocols/facebook/facebook.c',
    'pidgin/libpurple/protocols/facebook/http.c',
    'pidgin/libpurple/protocols/facebook/json.c',
    'pidgin/libpurple/protocols/facebook/mqtt.c',
    'pidgin/libpurple/protocols/facebook/thrift.c',
    'pidgin/libpurple/protocols/facebook/util.c',
    'pidgin/libpurple/http.c',
    'pidgin/libpurple/purple-socket.c'
)

# Determine the plugin directory if not provided by the user
if plugin_dir == ''
    purple_plugindir = ''
    if purple_dep.found()
        purple_plugindir = purple_dep.get_pkgconfig_variable('plugindir')
    endif
    if purple_plugindir == ''
        purple_plugindir = join_paths(get_option('libdir'), 'purple-2')
    endif
else
    purple_plugindir = plugin_dir
endif

# Define library
#plugin_library = shared_library('purple-facebook-plugin', source_files, dependencies : [glib_dep, json_dep, purple_dep, zlib_dep], c_args: compiler_options)

plugin_library = shared_library('facebook', source_files, dependencies : [glib_dep, json_dep, purple_dep, zlib_dep], c_args: compiler_options, install: true, install_dir: purple_plugindir)
