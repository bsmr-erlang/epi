import os

####################################################################################
# Command line options
#
opts = Options('custom.py')
opts.AddOptions(
	('CXX', 'The C++ compiler command'),
	("EXTRA_INCLUDE_PATH", "extra include paths",""),
	("EXTRA_LIBRARY_PATH", "extra library search paths",""),
	("EXTRA_CPP_FLAGS", "extra flags for c++ compiler",""),
	("EXTRA_LINK_FLAGS", "extra flags for linker",""),
	("EXTRA_LIBS", "extra libraries to include at link time",""),
	("PREFIX", "Install prefix", "/usr/local/"),
	PackageOption('EI_INC', 'Path to include headers of EI library',  0),
	PackageOption('EI_LIB', 'Path to EI library',  0),
	PackageOption('OPENTHREADS_INC', 'Path to include headers of openthreads library', 0),
	PackageOption('OPENTHREADS_LIB', 'Path to openthreads library', 0),
	PackageOption('LIBCWD_INC', 'Path to include headers of libcwd library', 0),
	PackageOption('LIBCWD_LIB', 'Path to libcwd library', 0),
#TODO:	BoolOption('USE_LIBCWD', 'Use libcwd library or not (only in debug build)', 1)
	BoolOption('debug', 'debug build', 1)
	)

####################################################################################
# CPPFLAGS
DEFAULT_CPPFLAGS = Split('-Wall -Wno-ctor-dtor-privacy -Wno-long-long')
OPT_CPPFLAGS = Split('-fno-rtti -ansi -pedantic')
DEBUG_CPPFLAGS = Split('-DCWDEBUG -g -fno-inline')

####################################################################################
# Default environment (using os environment)
env = Environment(options=opts, ENV = os.environ)
				  
debug = env.get('debug',0)

env.Append(CPPPATH = Split(env["EXTRA_INCLUDE_PATH"]))
env.Append(CPPFLAGS = Split(env["EXTRA_CPP_FLAGS"]))
env.Append(LINKFLAGS = Split(env["EXTRA_LINK_FLAGS"]))
env.Append(LIBPATH = Split(env["EXTRA_LIBRARY_PATH"]))
env.Append(LIBS = Split(env["EXTRA_LIBS"]))

if debug:
	env.Append(CPPPATH = ['${EI_INC}', '${OPENTHREADS_INC}', '${LIBCWD_INC}'])
	env.Append(LIBPATH = ['${EI_LIB}', '${OPENTHREADS_LIB}', '${LIBCWD_LIB}'])
	env.Append(CPPFLAGS = DEBUG_CPPFLAGS + DEFAULT_CPPFLAGS)
	env.Append(LIBS = ['ei', 'openthreads', 'cwd'])
else:
	env.Append(CPPPATH = ['${EI_INC}', '${OPENTHREADS_INC}'])
	env.Append(LIBPATH = ['${EI_LIB}', '${OPENTHREADS_LIB}'])
	env.Append(CPPFLAGS = OPT_CPPFLAGS + DEFAULT_CPPFLAGS)
	env.Append(LIBS = ['ei', 'openthreads'])

env.Append(CPPPATH = Dir('./include'))
env.Append(LIBPATH = Dir('./lib'))

###################################################################################
# Documentation
doxygen_doc = env.Command('docs/index.html', 'Doxyfile', 'doxygen $SOURCES')
Alias('doc', doxygen_doc)

###################################################################################
Help(opts.GenerateHelpText(env))

Export('env')	

SConscript('src/SConstruct')
SConscript('test/src/SConstruct')
SConscript('test/performance/SConstruct')
SConscript('sample/SConstruct')
