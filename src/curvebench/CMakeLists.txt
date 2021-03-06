include_directories(
  ${PROJECT_SOURCE_DIR}/src/chayai/src
  ${PROJECT_SOURCE_DIR}/src/ed25519-donna
  ${PROJECT_SOURCE_DIR}/src/secp256k1/src
)

set(CMAKE_C_FLAGS "-std=c99 -m64 -O3")
set(CMAKE_CXX_FLAGS "-std=c++11 -m64 -O3 -fpermissive")
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR})

add_custom_command(
  OUTPUT ${PROJECT_SOURCE_DIR}/src/secp256k1/src/field_5x52_asm.o
  COMMAND ${PROJECT_SOURCE_DIR}/src/secp256k1/nasm_lt.sh yasm -f elf64 -I. -I. ${PROJECT_SOURCE_DIR}/src/secp256k1/src/field_5x52_asm.asm  -fPIC -DPIC -o ${PROJECT_SOURCE_DIR}/src/secp256k1/src/field_5x52_asm.o)

add_executable(benchmark
  main.c

  secp256k1.c
  ${PROJECT_SOURCE_DIR}/src/secp256k1/src/field_5x52_asm.o

  ed25519.c
  ${PROJECT_SOURCE_DIR}/src/ed25519-donna/ed25519.c
)

target_link_libraries(benchmark
  chayai
  ssl
  crypto
)
