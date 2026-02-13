#-----------------------------------------------------------------------------
set(MODULE_NAME CfdMeshGenerator)

#-----------------------------------------------------------------------------
set(MODULE_PYTHON_SCRIPTS
  ${MODULE_NAME}.py
  ${MODULE_NAME}Lib/__init__.py
  ${MODULE_NAME}Lib/FTetWild.py
  ${MODULE_NAME}Lib/MeshingPipeline.py
  ${MODULE_NAME}Lib/MeshingWorker.py
  ${MODULE_NAME}Lib/Netgen.py
  )

set(MODULE_PYTHON_RESOURCES
  Resources/Icons/${MODULE_NAME}.png
  # Icons/ToggleEdges.svg is deliberately not listed here. Scripted module resources all land in
  # the shared qt-scripted-modules/Resources directory that resourcePath() resolves against, so
  # ClipVessel's identical copy is the one this module picks up at runtime. Listing it in both
  # modules makes two custom commands write the same output and CMake refuses to configure.
  Resources/Icons/ToggleClipping.svg
  Resources/UI/${MODULE_NAME}.ui
  )

#-----------------------------------------------------------------------------
slicerMacroBuildScriptedModule(
  NAME ${MODULE_NAME}
  SCRIPTS ${MODULE_PYTHON_SCRIPTS}
  RESOURCES ${MODULE_PYTHON_RESOURCES}
  WITH_GENERIC_TESTS
  )

#-----------------------------------------------------------------------------
if(BUILD_TESTING)

  # Register the unittest subclass in the main script as a ctest.
  # Note that the test will also be available at runtime.
  slicer_add_python_unittest(SCRIPT ${MODULE_NAME}.py)

  # Additional build-time testing
  add_subdirectory(Testing)
endif()
