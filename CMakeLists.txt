#
# project: fips-crunch
#

if (NOT FIPS_IMPORT)
    cmake_minimum_required(VERSION 2.8)
    get_filename_component(FIPS_ROOT_DIR "../fips" ABSOLUTE)
    include("${FIPS_ROOT_DIR}/cmake/fips.cmake")
    fips_setup()
    fips_project(fips-crunch)
endif()


fips_begin_lib(crnlib)
    include_directories(crunch/inc crunch/crnlib)
    fips_dir(crunch/crnlib)
    fips_files(
        crn_arealist.cpp
        crn_assert.cpp
        crn_checksum.cpp
        crn_colorized_console.cpp
        crn_command_line_params.cpp
        crn_comp.cpp
        crn_console.cpp
        crn_core.cpp
        crn_data_stream.cpp
        crn_mipmapped_texture.cpp
        crn_decomp.cpp
        crn_dxt1.cpp
        crn_dxt5a.cpp
        crn_dxt.cpp
        crn_dxt_endpoint_refiner.cpp
        crn_dxt_fast.cpp
        crn_dxt_hc_common.cpp
        crn_dxt_hc.cpp
        crn_dxt_image.cpp
        crn_dynamic_string.cpp
        crn_file_utils.cpp
        crn_find_files.cpp
        crn_hash.cpp
        crn_hash_map.cpp
        crn_huffman_codes.cpp
        crn_image_utils.cpp
        crnlib.cpp
        crn_math.cpp
        crn_mem.cpp
        crn_pixel_format.cpp
        crn_platform.cpp
        crn_prefix_coding.cpp
        crn_qdxt1.cpp
        crn_qdxt5.cpp
        crn_rand.cpp
        crn_resample_filters.cpp
        crn_resampler.cpp
        crn_ryg_dxt.cpp
        crn_sparse_bit_array.cpp
        crn_stb_image.cpp
        crn_strutils.cpp
        crn_symbol_codec.cpp
        crn_texture_file_types.cpp
        crn_threaded_resampler.cpp
        crn_threading_pthreads.cpp
        crn_timer.cpp
        crn_utils.cpp
        crn_value.cpp
        crn_vector.cpp
        crn_zeng.cpp
        crn_texture_comp.cpp
        crn_texture_conversion.cpp
        crn_dds_comp.cpp
        crn_lzma_codec.cpp
        crn_ktx_texture.cpp
        crn_etc.cpp
        crn_rg_etc1.cpp
        crn_miniz.cpp
        crn_jpge.cpp
        crn_jpgd.cpp
        lzma_7zBuf2.cpp
        lzma_7zBuf.cpp
        lzma_7zCrc.cpp
        lzma_7zFile.cpp
        lzma_7zStream.cpp
        lzma_Alloc.cpp
        lzma_Bcj2.cpp
        lzma_Bra86.cpp
        lzma_Bra.cpp
        lzma_BraIA64.cpp
        lzma_LzFind.cpp
        lzma_LzFindMt.cpp
        lzma_Threads.cpp
        lzma_LzmaDec.cpp
        lzma_LzmaEnc.cpp
        lzma_LzmaLib.cpp
    )

    if (FIPS_WINDOWS)
        fips_files(crn_threading_win32.cpp)
    endif()

    if (FIPS_LINUX)
        fips_files(crn_threading_pthreads.cpp)
    endif()

    if (FIPS_MACOS)
        fips_files(crn_threading_pthreads.cpp)
    endif()
fips_end_lib()

if (FIPS_MACOS OR FIPS_LINUX OR FIPS_WINDOWS)
    fips_begin_app(crunch cmdline)
        include_directories(crunch/crnlib crunch/inc)
        fips_dir(crunch/crunch)
        fips_files(crunch.cpp corpus_gen.cpp corpus_test.cpp)
        fips_deps(crnlib)

        if (FIPS_LINUX)
            fips_libs(pthread)
        endif()

        if (FIPS_MACOS)
            fips_libs(pthread)
        endif()
    fips_end_app()
endif()

if (NOT FIPS_IMPORT)
    if (FIPS_MACOS OR FIPS_LINUX OR FIPS_WINDOWS)
        fips_begin_app(example1 cmdline)
            include_directories(crunch/crnlib crunch/inc crunch/example1)
            fips_dir(crunch/example1)
            fips_files(example1.cpp)
            fips_deps(crnlib)

            if (FIPS_LINUX)
                fips_libs(pthread)
            endif()

            if (FIPS_MACOS)
                fips_libs(pthread)
            endif()
        fips_end_app()
        fips_begin_app(example2 cmdline)
            include_directories(crunch/crnlib crunch/inc crunch/example2)
            fips_dir(crunch/example2)
            fips_files(example2.cpp timer.cpp)
            fips_deps(crnlib)

            if (FIPS_LINUX)
                fips_libs(pthread)
            endif()

            if (FIPS_MACOS)
                fips_libs(pthread)
            endif()
        fips_end_app()
        fips_begin_app(example3 cmdline)
            include_directories(crunch/crnlib crunch/inc crunch/example3)
            fips_dir(crunch/example3)
            fips_files(example3.cpp)
            fips_deps(crnlib)

            if (FIPS_LINUX)
                fips_libs(pthread)
            endif()

            if (FIPS_MACOS)
                fips_libs(pthread)
            endif()
        fips_end_app()
    endif()
endif()

if (FIPS_CLANG)
    set_target_properties(crunch PROPERTIES COMPILE_FLAGS "-fomit-frame-pointer -ffast-math -fno-math-errno -g -fno-strict-aliasing -Wall -Wno-unused-value -Wno-unused -march=core2")
elseif(FIPS_GCC)
    set_target_properties(crunch PROPERTIES COMPILE_FLAGS "-fomit-frame-pointer -ffast-math -fno-math-errno -g -fno-strict-aliasing -Wall -Wno-unused-value -Wno-unused -march=core2")
elseif(FIPS_MSVC)
    set_target_properties(crunch PROPERTIES COMPILE_FLAGS "/wd4267 /wd4152 /wd4204 /wd4242 /wd4244 /wd4668 /wd4996")
endif()

if (NOT FIPS_IMPORT)
    fips_finish()
endif()

