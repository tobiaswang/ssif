project(
    'ssif', 
    'c',
    'cpp',
    default_options: [
          'c_std=c11',
          'cpp_std=c++17'
        ],
    version: '0.1',
)

boost_args = [
        '-DBOOST_ASIO_DISABLE_THREADS',
        '-DBOOST_ALL_NO_LIB',
        '-DBOOST_SYSTEM_NO_DEPRECATED',
        '-DBOOST_ERROR_CODE_HEADER_ONLY',
        '-DBOOST_COROUTINES_NO_DEPRECATION_WARNING'
]

cpp = meson.get_compiler('cpp')

boost_asio_dep = dependency('boost', modules : ['system', 'coroutine', 'date_time', 'thread'])
sdbusplus_dep = dependency('sdbusplus')
systemd_dep = dependency('libsystemd')
cjson_dep = dependency('libcjson')
systemd = dependency('systemd')

deps = [
        boost_asio_dep,
        sdbusplus_dep,
        systemd_dep,
        cjson_dep
]

systemd_system_unit_dir = systemd.get_pkgconfig_variable(
    'systemdsystemunitdir',
    define_variable: ['prefix', get_option('prefix')])

configure_file(
    copy: true,
    input: 'ssif-host.service',
    install: true,
    install_dir: systemd_system_unit_dir,
    output: 'ssif-host.service'
)

configure_file(
    copy: true,
    input: 'ssif-slave.service',
    install: true,
    install_dir: systemd_system_unit_dir,
    output: 'ssif-slave.service'
)

configure_file(
    copy: true,
    input: 'ssif.json',
    install: true,
    install_dir: '/usr/share/ssif/',
    output: 'ssif.json'
)

executable(
    'ssif-host', 
    'ssif_host_main.cpp',  
    'ssifhost.c',     
    'ssif_json.c', 
    cpp_args: boost_args,
    dependencies: deps,
    install:true,
    install_dir: get_option('bindir')
)

executable(
    'ssif-slave', 
    'ssif_slave_main.c',
    'ssif_json.c', 
    dependencies: deps,
    install:true,
    install_dir: get_option('bindir')
)