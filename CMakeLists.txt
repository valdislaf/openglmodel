
cmake_minimum_required(VERSION 3.22.3)
set(name "openglmodel")
project(${name})

if (MSVC)
    set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
endif()
#[[
if (CMAKE_GENERATOR MATCHES "Visual Studio")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /MTd")
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MTd")
    set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} /MTd")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /MTd")
endif()
if (MSVC)
    foreach (flag CMAKE_C_FLAGS CMAKE_CXX_FLAGS)
        if (${flag} MATCHES "/MT")
            string(REGEX REPLACE "/MT" "/MTd" ${flag} "${${flag}}")
        endif()
        if (${flag} MATCHES "/MTd")
            string(REGEX REPLACE "/MTd" "/MTd" ${flag} "${${flag}}")
        endif()
         if (${flag} MATCHES "/MD")
            string(REGEX REPLACE "/MD" "/MTd" ${flag} "${${flag}}")
        endif()
         if (${flag} MATCHES "/MDd")
            string(REGEX REPLACE "/MDd" "/MTd" ${flag} "${${flag}}")
        endif()
    endforeach()
endif()
]]


if(UNIX AND NOT APPLE)
    # Здесь указываем команды, которые должны быть выполнены только на Linux.
    set(GLUT_LIBRARY "/usr/lib/x86_64-linux-gnu/libglut.so")
    find_package(OpenGL REQUIRED)
    find_package(OpenGL REQUIRED)
    find_package(glfw3 REQUIRED)
    find_package(GLEW REQUIRED)

    # Задаем путь до директории с SOIL
    set(SOIL_DIR "${CMAKE_CURRENT_SOURCE_DIR}/soil")
    #set(SOIL_INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/soil/inc")
    #set(SOIL_INCLUDE_DIRS "${SOIL_DIR}/src")
    set(SOIL_LIBRARY_DIRS "${SOIL_DIR}/build")
    set(SOIL_LIBRARIES "soil")
    include_directories("${CMAKE_CURRENT_SOURCE_DIR}/soil/inc")
    #include_directories("${CMAKE_CURRENT_SOURCE_DIR}/soil/src")
    link_directories("${CMAKE_CURRENT_SOURCE_DIR}/build")
    link_directories(${SOIL_LIBRARY_DIRS})
    #BULLET3
    set(BULLET3_DIR "${CMAKE_CURRENT_SOURCE_DIR}/bullet3")
    set(BULLET3_INCLUDE_DIRS "${BULLET3_DIR}/src")
    # Add Bullet3 include directories
    include_directories(${BULLET3_INCLUDE_DIRS})
    # Set Bullet3 library directories
    set(BULLET3_LIBRARY_DIRS "${BULLET3_DIR}/ubuntu")
    # Add Bullet3 library directories
    link_directories(${BULLET3_LIBRARY_DIRS})

    find_package(PkgConfig REQUIRED)
    pkg_check_modules(GTK3 REQUIRED gtk+-3.0)
    find_package(Freetype REQUIRED)
    # Add this line to link against GLUT
    find_package(GLUT REQUIRED)
    #find_package(glm REQUIRED)

    include_directories(${OPENGL_INCLUDE_DIRS} ${GLEW_INCLUDE_DIRS} ${GLFW_INCLUDE_DIRS} ${GTK3_INCLUDE_DIRS} ${GLUT_INCLUDE_DIRS})
    link_directories(${GTK3_LIBRARY_DIRS})
    include_directories(${CMAKE_CURRENT_SOURCE_DIR})
    link_directories(${CMAKE_CURRENT_SOURCE_DIR})
    link_directories(${CMAKE_CURRENT_SOURCE_DIR}/lib-mingw-w64)
    link_directories(${CMAKE_CURRENT_SOURCE_DIR}/lib-vc2022)
    add_library(glad "${CMAKE_CURRENT_SOURCE_DIR}/glad.c")
    add_executable(${name} main.cpp)
    target_link_libraries(${name} ${OPENGL_LIBRARIES} glfw GLEW ${GTK3_LIBRARIES})
    target_link_libraries(${name} ${OPENGL_LIBRARIES} ${GLEW_LIBRARIES} ${GLFW_LIBRARIES} ${GTK3_LIBRARIES} ${GLUT_LIBRARY})
    target_link_libraries(${name} ${OPENGL_LIBRARIES} glfw GLEW ${GTK3_LIBRARIES} ${GLUT_LIBRARY})
    target_link_libraries(${name} glfw3dll)
    target_link_libraries(${name} glad)
    #target_link_libraries(${name}  glm)
    target_link_libraries(${name}  GLEW)
    target_link_libraries(${name}  freetype)
    target_link_libraries(${name} soil)
    target_link_libraries(${name}
            ${BULLET3_LIBRARY_DIRS}/libBulletCollision.a
            ${BULLET3_LIBRARY_DIRS}/libBulletDynamics.a
            ${BULLET3_LIBRARY_DIRS}/libLinearMath.a
            )
elseif(WIN32)
    # Здесь указываем команды, которые должны быть выполнены только на Windows.
    set(CMAKE_CXX_STANDARD 17)
    #find_package(OpenGL REQUIRED)
    #find_package(GLEW REQUIRED)
    #find_package(glfw3 REQUIRED)
    #find_package(GLEW REQUIRED)
    # Добавление пути до папки glm
    #include_directories("C:/glm-0.9.9.8")
    # Set the GLEW_ROOT_DIR
    set(GLEW_ROOT_DIR "${CMAKE_CURRENT_SOURCE_DIR}/glew-2.2.0.2.2.0.1/build/native")
    include_directories(${GLEW_ROOT_DIR}/include)
    set(GLFW_ROOT_DIR "${CMAKE_CURRENT_SOURCE_DIR}/glfw.3.3.8/build/native")
    include_directories(${GLEW_ROOT_DIR}/include)


    # link_directories(${GLEW_ROOT_DIR}/lib/Release/x64)
    # set(GLFW_ROOT_DIR "${CMAKE_CURRENT_SOURCE_DIR}/glfw.3.3.8/build/native")
    # link_directories(${GLFW_ROOT_DIR}/lib/static/v143/x64)
    if (CMAKE_GENERATOR MATCHES "Visual Studio")
       link_directories(${CMAKE_CURRENT_SOURCE_DIR}/soil/buildVS/Debug)
    else()
        link_directories(${CMAKE_CURRENT_SOURCE_DIR}/soil)
    endif()

    include_directories(${CMAKE_CURRENT_SOURCE_DIR})
    link_directories(${CMAKE_CURRENT_SOURCE_DIR})
    link_directories(${CMAKE_CURRENT_SOURCE_DIR}/soil)
   # link_directories(${CMAKE_CURRENT_SOURCE_DIR}/soil/buildVS/Debug)
    link_directories(${CMAKE_CURRENT_SOURCE_DIR}/lib-mingw-w64)
    link_directories(${CMAKE_CURRENT_SOURCE_DIR}/lib-vc2022)
    add_library(glad "${CMAKE_CURRENT_SOURCE_DIR}/glad.c")

    set(FREEGLUT_ROOT_PATH "${CMAKE_CURRENT_SOURCE_DIR}/freeglut")
    set(FREEGLUT_INCLUDE_DIR "${FREEGLUT_ROOT_PATH}/include")
    set(FREEGLUT_LIB_DIR "${FREEGLUT_ROOT_PATH}/build/lib/Debug")

    include_directories(${FREEGLUT_INCLUDE_DIR})
    link_directories(${FREEGLUT_LIB_DIR})

    set(FREETYPE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/freetype")
    set(FREETYPE_INCLUDE_DIRS "${FREETYPE_DIR}/include")
    set(FREETYPE_LIBRARY_DIRS "${FREETYPE_DIR}/objs/x64/Debug")
    set(FREETYPE_LIBRARIES "freetype")

    #BULLET3
#[[    set(BULLET3_DIR "${CMAKE_CURRENT_SOURCE_DIR}/bullet3")
    set(BULLET3_DIR_INCLUDE_DIRS "${BULLET3_DIR}/src")
    include_directories(${BULLET3_DIR_INCLUDE_DIRS})
    link_directories(${BULLET3_DIR_INCLUDE_DIRS})
    set(BULLET3_LIBRARY_DIRS "${BULLET3_DIR}/LIB_VS/lib/Debug")
    link_directories(${BULLET3_LIBRARY_DIRS})
    include_directories(${BULLET3_DIR_INCLUDE_DIRS})
    link_directories(${BULLET3_DIR_INCLUDE_DIRS})
    set(BULLET3_LIBRARY_DIRS "${BULLET3_DIR}/LIB_MINGW/lib")
    link_directories(${BULLET3_LIBRARY_DIRS})]]

    # BULLET3
    set(BULLET3_DIR "${CMAKE_CURRENT_SOURCE_DIR}/bullet3")
    set(BULLET3_INCLUDE_DIRS "${BULLET3_DIR}/src")
    # Add Bullet3 include directories
    include_directories(${BULLET3_INCLUDE_DIRS})
    # Set Bullet3 library directories
    if (CMAKE_GENERATOR MATCHES "Visual Studio")
        set(BULLET3_LIBRARY_DIRS "${BULLET3_DIR}/LIB_VS/lib/Debug")
    else()
        #set(BULLET3_LIBRARY_DIRS "${BULLET3_DIR}/LIB_VS/lib/Debug")
        set(BULLET3_LIBRARY_DIRS "${BULLET3_DIR}/LIB_MINGW/lib")
    endif()
    # Add Bullet3 library directories
    link_directories(${BULLET3_LIBRARY_DIRS})

    # Задаем путь до директории с SOIL
    set(SOIL_DIR "${CMAKE_CURRENT_SOURCE_DIR}/soil")
    # Задаем путь до директории с заголовочными файлами SOIL
    set(SOIL_INCLUDE_DIRS "${SOIL_DIR}/inc")
    set(SOIL_INCLUDE_DIRS "${SOIL_DIR}/src")
    # Задаем путь до директории с библиотеками SOIL
    set(SOIL_LIBRARY_DIRS "${SOIL_DIR}/Debug")
    set(SOIL_LIBRARY_DIRS "${SOIL_DIR}/cmake-build-debug")
    # Задаем список библиотек для линковки
    set(SOIL_LIBRARIES "soil")
    # Добавляем директорию с заголовочными файлами в список путей для поиска заголовочных файлов
    include_directories(${SOIL_INCLUDE_DIRS})
    # Добавляем директорию с библиотеками в список путей для поиска библиотек
    link_directories(${SOIL_LIBRARY_DIRS})
        if (CMAKE_GENERATOR MATCHES "Visual Studio")
                find_library(SOIL_LIBRARY soil PATHS ${CMAKE_SOURCE_DIR}/soil/buildVS/Debug)
                if (NOT SOIL_LIBRARY)
                    message(FATAL_ERROR "SOIL library not found")
                endif()
        else()
               find_library(SOIL_LIBRARY soil PATHS ${CMAKE_SOURCE_DIR}/soil/cmake-build-debug)
               #find_library(SOIL_LIBRARY soil PATHS ${CMAKE_SOURCE_DIR}/soil/buildVS/Debug)
               if (NOT SOIL_LIBRARY)
                   message(FATAL_ERROR "SOIL library not found")
               endif()
        endif()
    include_directories(${FREETYPE_INCLUDE_DIRS})
    link_directories(${FREETYPE_LIBRARY_DIRS})
    include_directories("${CMAKE_CURRENT_SOURCE_DIR}/soil/inc")
    #link_directories("C:/glut/lib")
    # include_directories("${CMAKE_CURRENT_SOURCE_DIR}/glew/include")
    #set(GLEW_LIB_DIR "${CMAKE_CURRENT_SOURCE_DIR}/glew/lib/Release/x64")
    #include_directories("C:/freetype/include")
    add_executable(${name} main.cpp)

    target_link_libraries(${name}  ${GLEW_ROOT_DIR}/lib/Release/x64/glew32.lib)
    target_link_libraries(${name}  ${GLFW_ROOT_DIR}/lib/dynamic/v143/x64/glfw3dll.lib)
    # target_include_directories(${name} PRIVATE ${GLEW_INCLUDE_DIRS} ${glfw3_INCLUDE_DIRS})
    #target_link_libraries(${name} PRIVATE ${GLEW_ROOT_DIR}/lib/Debug/x64 glfw)




    # Link against the GLEW library using keyword signature
    #target_link_libraries(${name} ${GLEW_ROOT_DIR}/lib/Debug/x64/glew32d.lib)
    #  target_link_libraries(${name} ${GLFW_ROOT_DIR}/lib/static/v143/x64/glfw3.lib)

    #target_compile_options(${name} PRIVATE /RTC1)

    #target_link_libraries(${name} ${GLFW_ROOT_DIR}/lib/Debug/x64/glfw3dll.lib)
    #target_link_libraries(${name}  glfw3)
    #target_link_libraries(${name}  glfw3dll)


    target_link_libraries(${name} ${FREEGLUT_LIB_DIR}/freeglutd.lib)
    target_link_libraries(${name} ${FREEGLUT_LIB_DIR}/freeglut_staticd.lib)
    #target_link_libraries(${name} Freetype)
            #"C:/freetype/objs"
    target_link_libraries(${name} ${FREETYPE_LIBRARIES})
    #target_link_libraries(${name} glut)
    #target_link_libraries(${name} glfw3dll)
    target_link_libraries(${name} glad)
    # target_link_libraries(${name} "${GLEW_LIB_DIR}/glew32.lib")
    # target_link_libraries(${name} "${GLEW_LIB_DIR}/glew32.lib")
    #  target_link_libraries(${name} "${GLEW_LIB_DIR}/glew32s.lib")
    # target_link_libraries(${name} glfw libglad.a C:/glew-2.1.0/lib/Release/x64/glew32s.lib C:/glew-2.1.0/lib/Release/x64/glew32.lib)
    target_link_libraries(${name} opengl32)
    #target_link_libraries(${name} "${SOIL_LIB_DIRS}/soil.lib")
    # Линкуем библиотеку SOIL
    target_link_libraries(${name} ${SOIL_LIBRARIES})
    #target_link_libraries(${name} ${BULLET3_LIBRARY_DIRS})
    if (CMAKE_GENERATOR MATCHES "Visual Studio")
        #set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MTd")
        target_link_libraries(${name}
                ${BULLET3_LIBRARY_DIRS}/BulletCollision_Debug.lib
                ${BULLET3_LIBRARY_DIRS}/BulletDynamics_Debug.lib
                ${BULLET3_LIBRARY_DIRS}/LinearMath_Debug.lib
                )
    else()
        target_link_libraries(${name}

                # ${BULLET3_LIBRARY_DIRS}/BulletCollision_Debug.lib
                # ${BULLET3_LIBRARY_DIRS}/BulletDynamics_Debug.lib
                # ${BULLET3_LIBRARY_DIRS}/LinearMath_Debug.lib
               ${BULLET3_LIBRARY_DIRS}/libBulletCollision_Debug.a
               ${BULLET3_LIBRARY_DIRS}/libBulletDynamics_Debug.a
               ${BULLET3_LIBRARY_DIRS}/libLinearMath_Debug.a
                )
    endif()
    # target_link_libraries(${name} ${BULLET_COLLISION_LIBRARIES})
    #add_executable(openglmodel main.cpp)
    find_package(OpenGL REQUIRED)
    target_link_libraries(openglmodel ${SOIL_LIBRARY})

    #find_package(GLEW REQUIRED)
    #include_directories(${GLEW_INCLUDE_DIRS})
    #target_link_libraries(YourExecutableName PRIVATE ${GLEW_LIBRARIES})

    find_library(SOIL_LIBRARY soil PATHS ${CMAKE_SOURCE_DIR}/soil/cmake-build-debug)
        if (NOT SOIL_LIBRARY)
            message(FATAL_ERROR "SOIL library not found")
        endif()
            target_link_libraries(openglmodel ${OPENGL_LIBRARIES} ${SOIL_LIBRARY})

    if (CMAKE_GENERATOR MATCHES "Visual Studio")
        # Копирование файлов для Visual Studio
        add_custom_command(TARGET ${name} POST_BUILD
                COMMAND ${CMAKE_COMMAND} -E copy_if_different
                "${CMAKE_CURRENT_SOURCE_DIR}/freeglutd.dll"
                "${CMAKE_CURRENT_SOURCE_DIR}/freeglutd.pdb"
                "${CMAKE_CURRENT_SOURCE_DIR}/glew32.dll"
                "${CMAKE_CURRENT_SOURCE_DIR}/glfw3.dll"
                "${CMAKE_CURRENT_SOURCE_DIR}/freetype.dll"
                "$<TARGET_FILE_DIR:${name}>"
                )
        add_custom_command(TARGET ${name} POST_BUILD
                COMMAND ${CMAKE_COMMAND} -E copy_directory
                "${CMAKE_CURRENT_SOURCE_DIR}/fonts"
                "$<TARGET_FILE_DIR:${name}>/fonts"
                )
        # Определите путь к шрифту относительно директории исходного кода
        set(FONT_PATH "fonts/arial.ttf")

        # Копирование шрифта в директорию сборки
        configure_file(
                ${CMAKE_CURRENT_SOURCE_DIR}/${FONT_PATH}
                ${CMAKE_CURRENT_BINARY_DIR}/${FONT_PATH}
                COPYONLY
        )
        file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/fonts DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
    else()
        # Копирование файлов для MinGW
        configure_file(${CMAKE_CURRENT_SOURCE_DIR}/freeglutd.dll ${CMAKE_CURRENT_BINARY_DIR}/freeglutd.dll COPYONLY)
        configure_file(${CMAKE_CURRENT_SOURCE_DIR}/freeglutd.pdb ${CMAKE_CURRENT_BINARY_DIR}/freeglutd.pdb COPYONLY)
        configure_file(${CMAKE_CURRENT_SOURCE_DIR}/glew32d.dll ${CMAKE_CURRENT_BINARY_DIR}/glew32.dll COPYONLY)
        configure_file(${CMAKE_CURRENT_SOURCE_DIR}/glfw3.dll ${CMAKE_CURRENT_BINARY_DIR}/glfw3.dll COPYONLY)
        configure_file(${CMAKE_CURRENT_SOURCE_DIR}/freetype.dll ${CMAKE_CURRENT_BINARY_DIR}/freetype.dll COPYONLY)
        file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/fonts DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
    endif()
endif()
