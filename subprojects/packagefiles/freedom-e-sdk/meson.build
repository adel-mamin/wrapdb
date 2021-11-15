project(
    'freedom-e-sdk',
    'c',
    version : '20.05.01.00',
    meson_version : '>= 0.52.0'
    license : ['Apache2', 'MIT']
)

bsp_subdir = get_option('bsp_subdir')
bsp_path = join_paths('bsp', bsp_subdir)

install_incdir = join_paths(
    meson.current_build_dir(), bsp_path, 'install', 'include')

install_headers(
    join_paths(bsp_path, 'metal.h'),
    join_paths(install_incdir, 'metal', 'machine.h')
)

install_headers(
    join_paths(bsp_path, 'metal-inline.h'),
    join_paths(install_incdir, 'metal', 'machine', 'inline.h')
)

install_headers(
    join_paths(bsp_path, 'metal-platform.h'),
    join_paths(install_incdir, 'metal', 'machine', 'platform.h')
)

c_args = [
    '-DPACKAGE_NAME="freedom-metal"',
    '-DPACKAGE_TARNAME="freedom-metal"',
    '-DPACKAGE_VERSION="v0.1.2"',
    '-DPACKAGE_STRING="freedom-metal v0.1.2"',
    '-DPACKAGE_BUGREPORT="https://github.com/sifive/freedom-metal/issues"',
    '-DPACKAGE_URL="https://github.com/sifive/freedom-metal"',
    '-DPACKAGE="freedom-metal"',
    '-DVERSION="v0.1.2"'
    '-DMTIME_RATE_HZ_DEF=10000000'
]

bsp_incdir = include_directories(bsp_path)

subdir('freedom-metal')

make_config = 'MTIME_RATE_HZ = @0@'.format(get_option('MTIME_RATE_HZ'))
make_config += 'HANDLE_INTERRUPT = @0@'.format(get_option('HANDLE_INTERRUPT'))
make_config += 'HANDLE_EXCEPTION = @0@'.format(get_option('HANDLE_EXCEPTION'))

if get_option('USING_MPU_WRAPPERS').enabled()
    make_config += 'USING_MPU_WRAPPERS = 1'
endif

MTIME_CTRL_ADDR = get_option('MTIME_CTRL_ADDR')
if MTIME_CTRL_ADDR >= 0
    make_config += 'MTIME_CTRL_ADDR = @0@'.format(MTIME_CTRL_ADDR)
endif

env = environment()
env.set('MAKE_CONFIG', make_config)
run_command(
    'python3',
    'FreeRTOS-metal/scripts/parser_auto_header.py',
    '--input_file',
    'FreeRTOS-metal/templates/Bridge_Freedom-metal_FreeRTOS.h.in',
    '--output_dir',
    install_incdir,
    check : true,
    env : env
)

freedom_metal_dep = declare_dependency(
    link_with : [libfreedom_metal, libfreedom_metal_gloss],
    include_directories : include_directories(install_incdir),
    version : meson.project_version()
)
