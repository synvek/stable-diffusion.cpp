if(MSVC)
    add_compile_options(/bigobj)
endif()


#set(TARGET sd)
#add_executable(${TARGET} main.cpp)
#if(MSVC)
#    target_compile_options(${TARGET} PRIVATE /bigobj)
#endif()
#install(TARGETS ${TARGET} RUNTIME)
#target_link_libraries(${TARGET} PRIVATE stable-diffusion ${CMAKE_THREAD_LIBS_INIT})
#target_compile_features(${TARGET} PUBLIC c_std_11 cxx_std_17)


set(TARGET_LIBARY synvek_backend_sd)

add_library(${TARGET_LIBARY} SHARED
        ../common/common.cpp
        ../common/log.cpp
        ../common/media_io.cpp
        image_metadata.cpp
        main.cpp
)
if(MSVC)
    target_compile_options(${TARGET_LIBARY} PRIVATE /bigobj)
endif()
install(TARGETS ${TARGET_LIBARY} RUNTIME)
target_link_libraries(${TARGET_LIBARY} PRIVATE stable-diffusion ${CMAKE_THREAD_LIBS_INIT})
target_compile_features(${TARGET_LIBARY} PUBLIC c_std_11 cxx_std_17)
if(APPLE)
    sd_set_macos_rpaths(${TARGET_LIBARY})
endif()
target_include_directories(${TARGET_LIBARY} PRIVATE
        "${CMAKE_CURRENT_SOURCE_DIR}/.."
        "${PROJECT_SOURCE_DIR}/src"
)
target_link_libraries(${TARGET_LIBARY} PRIVATE stable-diffusion zip ${CMAKE_THREAD_LIBS_INIT})
if(SD_WEBP)
    target_compile_definitions(${TARGET_LIBARY} PRIVATE SD_USE_WEBP)
    target_link_libraries(${TARGET_LIBARY} PRIVATE webp libwebpmux)
endif()
if(SD_WEBM)
    target_compile_definitions(${TARGET_LIBARY} PRIVATE SD_USE_WEBM)
    target_link_libraries(${TARGET_LIBARY} PRIVATE webm)
endif()
target_compile_features(${TARGET_LIBARY} PUBLIC c_std_11 cxx_std_17)


set(TARGET sd)
add_executable(${TARGET}
    ../common/common.cpp
    ../common/log.cpp
    ../common/media_io.cpp
    image_metadata.cpp
    main.cpp
)
if(APPLE)
    sd_set_macos_rpaths(${TARGET})
endif()
target_include_directories(${TARGET} PRIVATE
    "${CMAKE_CURRENT_SOURCE_DIR}/.."
    "${PROJECT_SOURCE_DIR}/src"
)
install(TARGETS ${TARGET} RUNTIME)
target_link_libraries(${TARGET} PRIVATE stable-diffusion zip ${CMAKE_THREAD_LIBS_INIT})
if(SD_WEBP)
    target_compile_definitions(${TARGET} PRIVATE SD_USE_WEBP)
    target_link_libraries(${TARGET} PRIVATE webp libwebpmux)
endif()
if(SD_WEBM)
    target_compile_definitions(${TARGET} PRIVATE SD_USE_WEBM)
    target_link_libraries(${TARGET} PRIVATE webm)
endif()
target_compile_features(${TARGET} PUBLIC c_std_11 cxx_std_17)
