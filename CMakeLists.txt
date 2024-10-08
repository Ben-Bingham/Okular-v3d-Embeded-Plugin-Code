remove_definitions(-DTRANSLATION_DOMAIN="okular")
add_definitions(-DTRANSLATION_DOMAIN="okular_poppler" -DGLM_FORCE_RIGHT_HANDED)

add_subdirectory( conf )

set(CMAKE_REQUIRED_LIBRARIES Poppler::Qt5 Qt5::Core Qt5::Gui)

include_directories(
   "3rdParty/V3D-Common" 
   "3rdParty/gzip-hpp/include"

   "../../core/" # ~/kde/src/okular/core

   "/usr/include/poppler/qt5"
   "/usr/include/tirpc" 
)

########### next target ###############

set(okularGenerator_poppler_PART_SRCS
   generator_pdf.cpp
   formfields.cpp
   annots.cpp
   pdfsignatureutils.cpp
   pdfsettingswidget.cpp
   imagescaling.cpp

   3rdParty/V3D-Common/Rendering/renderheadless.cpp
   3rdParty/V3D-Common/3rdParty/VulkanTools/VulkanTools.cpp
   3rdParty/V3D-Common/V3dFile/V3dFile.cpp
   3rdParty/V3D-Common/V3dFile/V3dObject.cpp
   3rdParty/V3D-Common/V3dFile/V3dObjects.cpp
   3rdParty/V3D-Common/V3dFile/V3dUtil.cpp
   3rdParty/V3D-Common/Utility/Arcball.cpp
   3rdParty/V3D-Common/Utility/ProtectedFunctionCaller.cpp
   3rdParty/V3D-Common/Utility/EventFilter.cpp
   3rdParty/V3D-Common/V3dModel.cpp
   3rdParty/V3D-Common/V3dModelManager.cpp
)

ki18n_wrap_ui(okularGenerator_poppler_PART_SRCS
   conf/pdfsettingswidget.ui
)

kconfig_add_kcfg_files(okularGenerator_poppler_PART_SRCS GENERATE_MOC conf/pdfsettings.kcfgc )

okular_add_generator(okularGenerator_poppler ${okularGenerator_poppler_PART_SRCS})

set(POPPLER_QT5_LIB /home/benjaminb/kde/src/okular/generators/Okular-v3d-Embeded-Plugin-Code/3rdParty/poppler/qt5/src/libpoppler-qt5.so)

target_link_libraries(okularGenerator_poppler okularcore KF5::I18n KF5::Completion KF5::KIOWidgets Qt5::Xml vulkan tirpc z Poppler::Qt5)

find_library(NAMES poppler-qt5)

########### autotests ###############

ecm_add_test(autotests/testimagescaling.cpp
    TEST_NAME "imageScalingTest"
    LINK_LIBRARIES Qt5::Test Qt5::Gui
)

########### install files ###############
install( FILES okularPoppler.desktop  DESTINATION  ${KDE_INSTALL_KSERVICES5DIR} )
install( PROGRAMS okularApplication_pdf.desktop org.kde.mobile.okular_pdf.desktop  DESTINATION  ${KDE_INSTALL_APPDIR} )
install( FILES org.kde.okular-poppler.metainfo.xml DESTINATION ${KDE_INSTALL_METAINFODIR} )
