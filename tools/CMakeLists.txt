set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)

set(rime_console_deps
  ${rime_library}
  ${rime_dict_library}
  ${rime_gears_library}
  ${rime_levers_library}
  ${rime_plugins_library})

set(rime_api_console_src "rime_api_console.cc")
add_executable(rime_api_console ${rime_api_console_src})
target_link_libraries(rime_api_console ${rime_console_deps})

set(rime_console_src "rime_console.cc")
add_executable(rime_console ${rime_console_src})
target_compile_definitions(rime_console PRIVATE RIME_IMPORTS)
target_link_libraries(rime_console ${rime_console_deps})

set(rime_deployer_src "rime_deployer.cc")
add_executable(rime_deployer ${rime_deployer_src})
target_compile_definitions(rime_deployer PRIVATE RIME_IMPORTS)
target_link_libraries(rime_deployer
  ${rime_library}
  ${rime_dict_library}
  ${rime_levers_library})

set(rime_dict_manager_src "rime_dict_manager.cc")
add_executable(rime_dict_manager ${rime_dict_manager_src})
target_link_libraries(rime_dict_manager
  ${rime_library}
  ${rime_dict_library}
  ${rime_levers_library})

set(rime_patch_src "rime_patch.cc")
add_executable(rime_patch ${rime_patch_src})
target_link_libraries(rime_patch
  ${rime_library}
  ${rime_levers_library})

install(TARGETS rime_deployer DESTINATION ${BIN_INSTALL_DIR})
install(TARGETS rime_dict_manager DESTINATION ${BIN_INSTALL_DIR})
install(TARGETS rime_patch DESTINATION ${BIN_INSTALL_DIR})

# do not work with Windows DLL; interfaces to dict are missing DLL export.
if(NOT WIN32 OR NOT BUILD_SHARED_LIBS)
  set(rime_table_decompiler_src "rime_table_decompiler.cc")
  add_executable(rime_table_decompiler ${rime_table_decompiler_src})
  target_compile_definitions(rime_deployer PRIVATE RIME_IMPORTS)
  target_link_libraries(rime_table_decompiler
    ${rime_library}
    ${rime_dict_library})

  install(TARGETS rime_table_decompiler DESTINATION ${BIN_INSTALL_DIR})
endif()
