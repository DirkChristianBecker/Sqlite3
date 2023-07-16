cmake_minimum_required(VERSION 3.7)

set(SQLITE3_LIB_NAME SQLite3)
project(${SQLITE3_LIB_NAME} VERSION 3.42.0 LANGUAGES C)

option(SQLITE_ENABLE_COLUMN_METADATA "enables column metadata"                         OFF)
option(SQLITE_ENABLE_DBSTAT_VTAB     "enables dbstat virtual table"                    OFF)
option(SQLITE_ENABLE_FTS3            "enables full text searches version 3"            OFF)
option(SQLITE_ENABLE_FTS4            "enables full text searches version 3 & 4"        OFF)
option(SQLITE_ENABLE_FTS5            "enables full text searches version 5"            OFF)
option(SQLITE_ENABLE_GEOPOLY         "enables Geopoly extention"                       OFF)
option(SQLITE_ENABLE_ICU             "enables international components for unicode"    OFF)
option(SQLITE_ENABLE_MATH_FUNCTIONS  "enables the built-in SQL math functions"         ON)
option(SQLITE_ENABLE_RBU             "enables resumable bulk update extension"         OFF)
option(SQLITE_ENABLE_RTREE           "enables R*TRee index extension"                  OFF)
option(SQLITE_ENABLE_STAT4           "enhances query planner under certain situations" OFF)
option(SQLITE_OMIT_DECLTYPE          "omit declared type of columns"                   ON)
option(SQLITE_OMIT_JSON              "disables JSON SQL functions"                     OFF)
option(SQLITE_RECOMMENDED_OPTIONS    "compile by SQLite3 recommended options"          ON)
option(SQLITE_USE_URI                "enables the default URI filename processing"     OFF)

## Find all source files to be compiled into object files
file(GLOB_RECURSE SQLITE_SOURCES 
	## Main
	${CMAKE_CURRENT_SOURCE_DIR}/*.c 
	${CMAKE_CURRENT_SOURCE_DIR}/*.h
)

## C++ compiler options
set(CMAKE_CXX_FLAGS "-Wall")
set(CMAKE_CXX_FLAGS_DEBUG "-Od -Ob0 -MTd")
set(CMAKE_CXX_FLAGS_RELEASE "-O3")

## 1. Compile...
add_library(${SQLITE3_LIB_NAME} STATIC ${SQLITE_SOURCES} ${SOURCES})

## Sources and headers
target_include_directories(${SQLITE3_LIB_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

## Properties
set_target_properties(${SQLITE3_LIB_NAME} PROPERTIES DEBUG_POSTFIX ${CMAKE_DEBUG_POSTFIX})

target_compile_options(
    ${SQLITE3_LIB_NAME} 
	PUBLIC 
    /W3 /nologo /EHsc
    "$<$<CONFIG:Debug>:/MTd;/Od;/Ob0;/Zi;/RTC1;/DDEBUG;/D_DEBUG;>"
    "$<$<CONFIG:Release>:/MT;/O2;/Ob2;/DNDEBUG;>"
)

target_compile_definitions(${SQLITE3_LIB_NAME} PUBLIC # inject user's options
    $<BUILD_INTERFACE:
        $<$<BOOL:${SQLITE_ENABLE_COLUMN_METADATA}>:SQLITE_ENABLE_COLUMN_METADATA>
        $<$<BOOL:${SQLITE_ENABLE_DBSTAT_VTAB}>:SQLITE_ENABLE_DBSTAT_VTAB>
        $<$<BOOL:${SQLITE_ENABLE_FTS3}>:SQLITE_ENABLE_FTS3>
        $<$<BOOL:${SQLITE_ENABLE_FTS4}>:SQLITE_ENABLE_FTS4>
        $<$<BOOL:${SQLITE_ENABLE_FTS5}>:SQLITE_ENABLE_FTS5>
        $<$<BOOL:${SQLITE_ENABLE_GEOPOLY}>:SQLITE_ENABLE_GEOPOLY>
        $<$<BOOL:${SQLITE_ENABLE_ICU}>:SQLITE_ENABLE_ICU>
        $<$<BOOL:${SQLITE_ENABLE_MATH_FUNCTIONS}>:SQLITE_ENABLE_MATH_FUNCTIONS>
        $<$<BOOL:${SQLITE_ENABLE_RBU}>:SQLITE_ENABLE_RBU>
        $<$<BOOL:${SQLITE_ENABLE_RTREE}>:SQLITE_ENABLE_RTREE>
        $<$<BOOL:${SQLITE_ENABLE_STAT4}>:SQLITE_ENABLE_STAT4>
        $<$<BOOL:${SQLITE_OMIT_DECLTYPE}>:SQLITE_OMIT_DECLTYPE>
        $<$<BOOL:${SQLITE_OMIT_JSON}>:SQLITE_OMIT_JSON>
        $<$<BOOL:${SQLITE_USE_URI}>:SQLITE_USE_URI>
        $<$<BOOL:${SQLITE_RECOMMENDED_OPTIONS}>:
            SQLITE_DEFAULT_MEMSTATUS=0
            SQLITE_DEFAULT_WAL_SYNCHRONOUS=1
            SQLITE_DQS=0
            SQLITE_LIKE_DOESNT_MATCH_BLOBS
            SQLITE_MAX_EXPR_DEPTH=0
            SQLITE_OMIT_DEPRECATED
            SQLITE_OMIT_PROGRESS_CALLBACK
            SQLITE_OMIT_SHARED_CACHE
            SQLITE_USE_ALLOCA
        >
    >
    )

## 3. Install...
install(TARGETS ${SQLITE3_LIB_NAME} RUNTIME DESTINATION bin)
configure_file(sqlite3_config.h.in ${CMAKE_BINARY_DIR}/sqlite3_config.h)

