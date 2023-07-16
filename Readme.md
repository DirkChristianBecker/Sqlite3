# SQLite Amalgamation
This repository mirrors the [SQLite](http://www.sqlite.org/download.html) amalgamation, which is the recommended method of building SQLite into larger projects.
It is also prepared to support `cmake` for building, installing and exporting and should be used as a submodule in a git project.

## usage
to integrate this library into your project simply add these lines to your project
`cmake`:
```cmake
add_subdirectory(SQLite3)
target_link_libraries(${PROJECT_NAME} SQLite3)
```

SQLite includes more than 100 files in `*.c` / `*.h`, but
> The [amalgamation](http://www.sqlite.org/amalgamation.html) contains
> everything you need to integrate SQLite into a larger project. Just copy the
> amalgamation into your source directory and compile it along with the other C
> code files in your project.
> ([A more detailed discussion](http://www.sqlite.org/howtocompile.html) of the
> compilation process is available.) You may also want to make use of
> the "sqlite3.h" header file that defines the programming API for SQLite. The
> sqlite3.h header file is available separately. The sqlite3.h file is also
> contained within the amalgamation, in the first few thousand lines. So if you
> have a copy of sqlite3.c but cannot seem to locate sqlite3.h, you can always
> regenerate the sqlite3.h by copying and pasting from the amalgamation.

![SQLite3](http://www.sqlite.org/images/sqlite370_banner.gif)


## SQLite3 build options
`SQLite3` comes with plenty of [compile options](https://www.sqlite.org/compile.html)

following cmake build options control some of those compile options:

| options                         | default |
| :--                             | :--     |
| `SQLITE_ENABLE_COLUMN_METADATA` | off     |
| `SQLITE_ENABLE_DBSTAT_VTAB`     | off     |
| `SQLITE_ENABLE_FTS3`            | off     |
| `SQLITE_ENABLE_FTS4`            | off     |
| `SQLITE_ENABLE_FTS5`            | off     |
| `SQLITE_ENABLE_GEOPOLY`         | off     |
| `SQLITE_ENABLE_ICU`             | off     |
| `SQLITE_ENABLE_MATH_FUNCTIONS`  | on      |
| `SQLITE_ENABLE_RBU`             | off     |
| `SQLITE_ENABLE_RTREE`           | off     |
| `SQLITE_ENABLE_STAT4`           | off     |
| `SQLITE_OMIT_DECLTYPE`          | on      |
| `SQLITE_OMIT_JSON`              | off     |
| `SQLITE_USE_URI`                | off     |


these **recommended** compile options are also passed to the compiler by
`SQLITE_RECOMMENDED_OPTIONS` (on by default):

| options                            |
| :--                                |
| SQLITE_DEFAULT_MEMSTATUS       = 0 |
| SQLITE_DEFAULT_WAL_SYNCHRONOUS = 1 |
| SQLITE_DQS                     = 0 |
| SQLITE_LIKE_DOESNT_MATCH_BLOBS     |
| SQLITE_MAX_EXPR_DEPTH          = 0 |
| SQLITE_OMIT_DECLTYPE               |
| SQLITE_OMIT_DEPRECATED             |
| SQLITE_OMIT_PROGRESS_CALLBACK      |
| SQLITE_OMIT_SHARED_CACHE           |
| SQLITE_USE_ALLOCA                  |
