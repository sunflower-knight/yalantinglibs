add_executable(benchmark_client client.cpp)
target_link_libraries(benchmark_client PRIVATE yalantinglibs::coro_rpc)
target_include_directories(benchmark_client PRIVATE . ..)


option(GENERATE_BENCHMARK_DATA "Generate benchmark data" ON)
message(STATUS "GENERATE_BENCHMARK_DATA: ${GENERATE_BENCHMARK_DATA}")
if (GENERATE_BENCHMARK_DATA)
    add_executable(benchmark_data_gen data_gen.cpp)
    target_link_libraries(benchmark_data_gen PRIVATE yalantinglibs::coro_rpc)
    target_include_directories(benchmark_data_gen PRIVATE . ..)
    target_compile_definitions(benchmark_data_gen PRIVATE GENERATE_BENCHMARK_DATA)
    add_custom_command(
            TARGET benchmark_data_gen POST_BUILD
            COMMAND ./benchmark_data_gen
            COMMENT "generate benchmark data"
            WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/benchmark
            VERBATIM
    )
endif ()
