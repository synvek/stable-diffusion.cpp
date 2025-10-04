if(MSVC)
    add_compile_options(/bigobj)
endif()


set(TARGET sd)


add_executable(${TARGET} main.cpp)
if(MSVC)
    target_compile_options(${TARGET} PRIVATE /bigobj)
endif()
install(TARGETS ${TARGET} RUNTIME)
target_link_libraries(${TARGET} PRIVATE stable-diffusion ${CMAKE_THREAD_LIBS_INIT})
target_compile_features(${TARGET} PUBLIC c_std_11 cxx_std_17)


set(TARGET_LIBARY synvek_backend_sd)

add_library(${TARGET_LIBARY} SHARED main.cpp)
if(MSVC)
    target_compile_options(${TARGET_LIBARY} PRIVATE /bigobj)
endif()
install(TARGETS ${TARGET_LIBARY} RUNTIME)
target_link_libraries(${TARGET_LIBARY} PRIVATE stable-diffusion ${CMAKE_THREAD_LIBS_INIT})
target_compile_features(${TARGET_LIBARY} PUBLIC c_std_11 cxx_std_17)
