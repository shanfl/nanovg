project(nanovg_demo)

macro(source_group_by_dir source_files)
    if(MSVC)
        set(sgbd_cur_dir ${CMAKE_CURRENT_SOURCE_DIR})
        foreach(sgbd_file ${${source_files}})
            string(REGEX REPLACE ${sgbd_cur_dir}/\(.*\) \\1 sgbd_fpath ${sgbd_file})
            string(REGEX REPLACE "\(.*\)/.*" \\1 sgbd_group_name ${sgbd_fpath})
            string(COMPARE EQUAL ${sgbd_fpath} ${sgbd_group_name} sgbd_nogroup)
            string(REPLACE "/" "\\" sgbd_group_name ${sgbd_group_name})
            if(sgbd_nogroup)
                set(sgbd_group_name "\\")
            endif(sgbd_nogroup)
            source_group(${sgbd_group_name} FILES ${sgbd_file})
        endforeach(sgbd_file)
    endif(MSVC)
endmacro(source_group_by_dir)

add_definitions(-DNANOVG_GLEW -DGLEW_STATIC)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/3rd/glfw3/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/3rd/glew/include)
message(STATUS "CMAKE_SOURCE_DIR = " ${CMAKE_SOURCE_DIR})
message(STATUS "nanovg_demo _source dir = " ${nanovg_demo_SOURCE_DIR})
message(STATUS "PROJECT_SOURCE_DIR _source dir = " ${PROJECT_SOURCE_DIR})
message(STATUS "CMAKE_CACHEFILE_DIR = " ${CMAKE_CACHEFILE_DIR})

link_directories(${CMAKE_CACHEFILE_DIR}/3rd/glfw3/src/Debug)

set(nanovg_src 
	./src/nanovg.h 
	./src/nanovg.c 
	./src/fontstash.h 
	./src/nanovg_gl.h
	./src/nanovg_gl_utils.h
	./src/stb_image.h
	./src/stb_truetype.h
	)


set(glew_src 
	${CMAKE_CURRENT_SOURCE_DIR}/3rd/glew/include/gl/glew.h
	${CMAKE_CURRENT_SOURCE_DIR}/3rd/glew/src/glew.c)

set(gl3_src 
	${nanovg_src}  
	${glew_src}
	./example/example_gl3.c
	./example/demo.h
	./example/demo.c
	./example/perf.h
	./example/perf.c
	./example/stb_image_write.h
)

add_subdirectory(3rd/glfw3)

source_group_by_dir(gl3_src)
	
add_executable(nanovg_ogl3 ${gl3_src})
target_link_libraries(nanovg_ogl3 glfw3 opengl32)
add_dependencies(nanovg_ogl3 glfw3 glfw)

	