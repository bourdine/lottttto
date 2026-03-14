cmake_minimum_required(VERSION 3.18.1)
project("lottttto-miner")

# Добавляем флаги в зависимости от архитектуры
if(CMAKE_ANDROID_ARCH_ABI STREQUAL "arm64-v8a")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -march=armv8-a+fp+simd")
endif()

# Базовая оптимизация для всех архитектур
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -fomit-frame-pointer -funroll-loops")

# Создаем нашу библиотеку-обертку
add_library(miner_jni SHARED native-lib.cpp)

# Находим системную библиотеку для логирования
find_library(log-lib log)

# Принудительно включаем мок-режим (без реальной библиотеки XMRig)
target_compile_definitions(miner_jni PRIVATE HAVE_XMRIG=0)

# Линкуем только с системными библиотеками
target_link_libraries(miner_jni ${log-lib})
