cmake_minimum_required(VERSION 2.8)

set(HEADERS_CRYPTO
    c_aes128.h
    blake2_impl.h
    blake2s.h
    c_blake2s256.h
    c_camellia128.h
    common.h
    c_crc32.h
    c_des.h
    c_gost.h
    c_haval5_256.h
    c_hmac_md5.h
    jtr_crc32.h
    jtr_gost.h
    jtr_sph_haval.h
    jtr_sph_skein.h
    jtr_sph_types.h
    keccak1600.h
    my_rand48_r.h
    my_time.h
    oneWayFunction.h
    PoW.h
    c_rc4.h
    c_ripemd160.h
    c_sha1.h
    c_sha3_256.h
    c_sha256.h
    c_sha512.h
    c_skein512_256.h
    c_whirlpool.h
    cryptoHello.h
   )

set(SOURCES_CRYPTO
    c_aes128.c
    blake2s.c
    c_blake2s256.c
    c_camellia128.c
    common.c
    c_crc32.c
    c_des.c
    c_gost.c
    keccak1600.c
    c_haval5_256.c
    c_hmac_md5.c
    jtr_crc32.c
    jtr_gost.c
    jtr_haval.c
    jtr_skein.c
    my_time.c
    oneWayFunction.c
    PoW.c
    c_rc4.c
    c_ripemd160.c
    c_sha1.c
    c_sha3_256.c
    c_sha256.c
    c_sha512.c
    c_skein512_256.c
    c_whirlpool.c
 #   jtr_haval_helper.c
	cryptoHello.cpp
   )

#SET(CMAKE_C_COMPILER gcc)
#SET(CMAKE_CXX_COMPILER g++)

include_directories(..)

if(UNIX)
    include_directories(/usr/include)
    link_directories("/usr/lib/x86_64-linux-gnu" "/usr/lib/gcc/x86_64-linux-gnu/5.4.0")
#elseif(WIN32)
    include_directories("c:/msys64/mingw64/include")
    link_directories("c:/msys64/mingw64/lib" "c:/msys64/mingw64/bin")
endif()

find_library(RUNTIME_LIB ssl)
find_library(RUNTIME_LIB crypto)
find_library(RUNTIME_LIB gomp)
link_libraries(${RUNTIME_LIB})

set(EXTRA_LIBS ssl crypto gomp)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99 -O3 -g -fopenmp")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O0 -ggdb -fopenmp")


add_library(cryptoHello ${HEADERS_CRYPTO} ${SOURCES_CRYPTO}) 
target_link_libraries(cryptoHello ${EXTRA_LIBS})

add_executable(PoW main.cpp ${HEADERS_CRYPTO} ${SOURCES_CRYPTO})
target_link_libraries(PoW ${EXTRA_LIBS})

