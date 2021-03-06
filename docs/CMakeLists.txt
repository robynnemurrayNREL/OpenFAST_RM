find_package(Doxygen)
find_package(Sphinx)

if(DOXYGEN_FOUND)
    configure_file(Doxyfile.in Doxyfile @ONLY)
    add_custom_target(doxygen
        COMMAND ${DOXYGEN_EXECUTABLE} "${CMAKE_CURRENT_BINARY_DIR}/Doxyfile" )
else()
    message(WARNING "Skipping Doxygen documentation due to missing dependencies.")
endif()

if(SPHINX_FOUND)
    set(SPHINX_GENERATOR html)
    file(COPY "conf.py" DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
    file(COPY "_static" DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
    file(COPY "openfastlogo.jpg" DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
    add_custom_target(sphinx
        COMMAND ${SPHINX_EXECUTABLE} -b ${SPHINX_GENERATOR} 
        -c ${CMAKE_CURRENT_BINARY_DIR}
        "${CMAKE_CURRENT_SOURCE_DIR}" "${CMAKE_CURRENT_BINARY_DIR}/${SPHINX_GENERATOR}"
        )
    add_dependencies(sphinx doxygen)
    add_custom_target(docs DEPENDS sphinx)
else()
    message(WARNING "Skipping Sphinx documentation due to missing dependencies.")
endif()
