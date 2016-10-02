cmake_minimum_required(VERSION 3.0)

set(PythonLibs_FIND_VERSION TRUE)
set(PythonLibs_FIND_VERSION_MAJOR 2)
find_package(PythonLibs REQUIRED)

include_directories(
    SYSTEM
    ${PYTHON_INCLUDE_DIR}
    ${GTK_DIR}/include
    ${GTK_DIR}/include/cairo
    ${GTK_DIR}/include/glib-2.0
    ${GTK_DIR}/lib/glib-2.0/include
    ${GTK_DIR}/include/pygtk-2.0
    ${GTK_DIR}/include/pango-1.0
    ${GTK_DIR}/include/atk-1.0
    ${GTK_DIR}/include/pycairo
    ${GTK_DIR}/include/gtk-2.0
    ${GTK_DIR}/lib/gtk-2.0/include
    ${GTK_DIR}/include/gdk-pixbuf-2.0
)

set(DEFINES HAVE_CONFIG_H=1 PY_SSIZE_T_CLEAN=1 FFI_BUILDING=1)
set(LIBS gdk_pixbuf-2.0 gdk-win32-2.0 gtk-win32-2.0 atk-1.0 pango-1.0 pangocairo-1.0 cairo gio-2.0 glib-2.0 gmodule-2.0 gobject-2.0 gthread-2.0 libffi.lib ${PYTHON_LIBRARY})
link_directories(${GTK_DIR}/lib)

set(MOO_PYTHON_LIB_DIR ${CMAKE_INSTALL_PREFIX}/bin/Lib/site-packages)

include_directories(${CMAKE_SOURCE_DIR})

#--------------------------------------------------------------------------------------------------

add_library(pango MODULE
    pango.c
    pangomodule.c
    pango.pyd.def
)

target_compile_definitions(pango PRIVATE ${DEFINES})
target_link_libraries(pango ${LIBS})
set_target_properties(pango PROPERTIES
    PREFIX ""
    SUFFIX ".pyd"
    VERSION "2.22.0")
install(TARGETS pango DESTINATION ${MOO_PYTHON_LIB_DIR}/gtk-2.0)

install(FILES
    pango.defs
    pango-types.defs
    pangocairo.defs
DESTINATION share/pygtk/2.0/defs)

#--------------------------------------------------------------------------------------------------

add_library(pangocairo MODULE 
    pangocairo.c
    pangocairomodule.c
    pangocairo.pyd.def
)

target_compile_definitions(pangocairo PRIVATE ${DEFINES})
target_link_libraries(pangocairo ${LIBS})
set_target_properties(pangocairo PROPERTIES
    PREFIX ""
    SUFFIX ".pyd"
    VERSION "2.22.0")
install(TARGETS pangocairo DESTINATION ${MOO_PYTHON_LIB_DIR}/gtk-2.0)

#--------------------------------------------------------------------------------------------------

add_library(atk MODULE 
    atk.c
    atkmodule.c
    atk.pyd.def
)

target_compile_definitions(atk PRIVATE ${DEFINES})
target_link_libraries(atk ${LIBS})
set_target_properties(atk PROPERTIES
    PREFIX ""
    SUFFIX ".pyd"
    VERSION "2.22.0")
install(TARGETS atk DESTINATION ${MOO_PYTHON_LIB_DIR}/gtk-2.0)

install(FILES
    atk.defs
    atk-types.defs
DESTINATION share/pygtk/2.0/defs)

#--------------------------------------------------------------------------------------------------

add_library(_gtk MODULE 
    gtk/gtk.c
    gtk/gdk.c
    gtk/gtk-types.c
    gtk/gtkmodule.c
    gtk/gtkobject-support.c
    gtk/pygtkcellrenderer.c
    gtk/pygtktreemodel.c
    gtk/_gtk.pyd.def
)

target_compile_definitions(_gtk PRIVATE ${DEFINES})
target_link_libraries(_gtk ${LIBS})
set_target_properties(_gtk PROPERTIES
    PREFIX ""
    SUFFIX ".pyd"
    VERSION "2.22.0")
install(TARGETS _gtk DESTINATION ${MOO_PYTHON_LIB_DIR}/gtk-2.0/gtk)
install(FILES
    gtk/__init__.py
    gtk/_lazyutils.py
    gtk/compat.py
    gtk/deprecation.py
    gtk/keysyms.py
DESTINATION ${MOO_PYTHON_LIB_DIR}/gtk-2.0/gtk)
install(FILES
    gtk/pygtk.h
DESTINATION ${GTK_DIR}/include/pygtk-2.0/pygtk)

install(FILES
    gtk/gtk-2.14-types.defs
    gtk/gtk-2.16.defs
    gtk/gtk-2.16-types.defs
    gtk/gtk-base.defs
    gtk/gtk-base-types.defs
    gtk/gtk-extrafuncs.defs
    gtk/gtk-types.defs
    gtk/gdk.defs
    gtk/gdk-2.10.defs
    gtk/gdk-2.12.defs
    gtk/gdk-2.14.defs
    gtk/gdk-2.16.defs
    gtk/gdk-base.defs
    gtk/gdk-base-types.defs
    gtk/gdk-types.defs
    gtk/gtk.defs
    gtk/gtk-2.10.defs
    gtk/gtk-2.10-types.defs
    gtk/gtk-2.12.defs
    gtk/gtk-2.12-types.defs
    gtk/gtk-2.14.defs
DESTINATION share/pygtk/2.0/defs)
