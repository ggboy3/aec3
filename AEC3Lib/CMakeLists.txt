cmake_minimum_required(VERSION 3.10)

# 项目信息
project(AEC3Demo)

set(CMAKE_C_COMPILER "/opt/rh/devtoolset-11/root/usr/bin/gcc")
set(CMAKE_CXX_COMPILER "/opt/rh/devtoolset-11/root/usr/bin/g++")


# 设置C++标准
#set(CMAKE_CXX_STANDARD 17)
#set(CMAKE_CXX_STANDARD_REQUIRED ON)
#set(CMAKE_CXX_EXTENSIONS OFF)

# 如果编译器无法自动识别C++20标准，可以手动指定标志
if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    add_compile_options(-std=c++20)
elseif(CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
    add_compile_options(-std=c++17)
elseif(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    add_compile_options(/std:c++17)
endif()

add_compile_options(-mfma)

# 设置编译选项
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    add_definitions(-D_DEBUG)
    add_definitions(-DWEBRTC_POSIX)
    add_definitions(-DWEBRTC_LINUX)
    add_definitions(-DNOMINMAX)
    add_definitions(-DAEC3WRAPPER_EXPORTS)
    add_definitions(-D_CRT_SECURE_NO_WARNINGS)
    #add_definitions(-DWIN32_LEAN_AND_MEAN)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra")
else()
    add_definitions(-DNDEBUG)
    add_definitions(-DWEBRTC_POSIX)
    add_definitions(-DWEBRTC_LINUX)
    add_definitions(-DNOMINMAX)
    add_definitions(-DAEC3WRAPPER_EXPORTS)
    add_definitions(-D_CRT_SECURE_NO_WARNINGS)
    #add_definitions(-DWIN32_LEAN_AND_MEAN)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
endif()

# 设置包含目录
include_directories(
    ${CMAKE_SOURCE_DIR}/../
    ${CMAKE_SOURCE_DIR}/../absl/base
    ${CMAKE_SOURCE_DIR}/../absl/memory
    ${CMAKE_SOURCE_DIR}/../absl/meta
    ${CMAKE_SOURCE_DIR}/../absl/strings
    ${CMAKE_SOURCE_DIR}/../absl/types
    ${CMAKE_SOURCE_DIR}/../absl/utility
    ${CMAKE_SOURCE_DIR}/../api
    ${CMAKE_SOURCE_DIR}/../common_audio
    ${CMAKE_SOURCE_DIR}/../modules/audio_processing
    ${CMAKE_SOURCE_DIR}/../rtc_base
)

# 源文件列表
set(SOURCES


    # 添加源文件 (.cc)
    ../absl/base/internal/throw_delegate.cc
    ../absl/strings/ascii.cc
    ../absl/strings/internal/memutil.cc
    ../absl/strings/string_view.cc
    ../absl/types/bad_optional_access.cc
    ../api/audio/audio_frame.cc
    ../api/audio/channel_layout.cc
    ../api/audio/echo_canceller3_config.cc
    ../api/audio/echo_canceller3_factory.cc
    ../api/audio/echo_detector_creator.cc
    ../common_audio/audio_util.cc
    ../common_audio/channel_buffer.cc
    ../common_audio/resampler/push_sinc_resampler.cc
    ../common_audio/resampler/sinc_resampler.cc
    ../common_audio/resampler/sinc_resampler_avx2.cc
    ../common_audio/resampler/sinc_resampler_sse.cc
    ../common_audio/signal_processing/splitting_filter.c
    ../common_audio/third_party/ooura/fft_size_128/ooura_fft.cc
    ../common_audio/third_party/ooura/fft_size_128/ooura_fft_sse2.cc
    ../modules/audio_processing/aec3/adaptive_fir_filter.cc
    ../modules/audio_processing/aec3/adaptive_fir_filter_avx2.cc
    ../modules/audio_processing/aec3/adaptive_fir_filter_erl.cc
    ../modules/audio_processing/aec3/adaptive_fir_filter_erl_avx2.cc
    ../modules/audio_processing/aec3/aec3_common.cc
    ../modules/audio_processing/aec3/aec3_fft.cc
    ../modules/audio_processing/aec3/aec_state.cc
    ../modules/audio_processing/aec3/alignment_mixer.cc
    ../modules/audio_processing/aec3/api_call_jitter_metrics.cc
    ../modules/audio_processing/aec3/block_buffer.cc
    ../modules/audio_processing/aec3/block_delay_buffer.cc
    ../modules/audio_processing/aec3/block_framer.cc
    ../modules/audio_processing/aec3/block_processor.cc
    ../modules/audio_processing/aec3/block_processor_metrics.cc
    ../modules/audio_processing/aec3/clockdrift_detector.cc
    ../modules/audio_processing/aec3/coarse_filter_update_gain.cc
    ../modules/audio_processing/aec3/comfort_noise_generator.cc
    ../modules/audio_processing/aec3/config_selector.cc
    ../modules/audio_processing/aec3/decimator.cc
    ../modules/audio_processing/aec3/dominant_nearend_detector.cc
    ../modules/audio_processing/aec3/downsampled_render_buffer.cc
    ../modules/audio_processing/aec3/echo_audibility.cc
    ../modules/audio_processing/aec3/echo_canceller3.cc
    ../modules/audio_processing/aec3/echo_path_delay_estimator.cc
    ../modules/audio_processing/aec3/echo_path_variability.cc
    ../modules/audio_processing/aec3/echo_remover.cc
    ../modules/audio_processing/aec3/echo_remover_metrics.cc
    ../modules/audio_processing/aec3/erle_estimator.cc
    ../modules/audio_processing/aec3/erl_estimator.cc
    ../modules/audio_processing/aec3/fft_buffer.cc
    ../modules/audio_processing/aec3/fft_data_avx2.cc
    ../modules/audio_processing/aec3/filter_analyzer.cc
    ../modules/audio_processing/aec3/frame_blocker.cc
    ../modules/audio_processing/aec3/fullband_erle_estimator.cc
    ../modules/audio_processing/aec3/matched_filter.cc
    #../modules/audio_processing/aec3/matched_filter_avx2.cc
    ../modules/audio_processing/aec3/matched_filter_lag_aggregator.cc
    ../modules/audio_processing/aec3/moving_average.cc
    ../modules/audio_processing/aec3/multi_channel_content_detector.cc
    ../modules/audio_processing/aec3/refined_filter_update_gain.cc
    ../modules/audio_processing/aec3/render_buffer.cc
    ../modules/audio_processing/aec3/render_delay_buffer.cc
    ../modules/audio_processing/aec3/render_delay_controller.cc
    ../modules/audio_processing/aec3/render_delay_controller_metrics.cc
    ../modules/audio_processing/aec3/render_signal_analyzer.cc
    ../modules/audio_processing/aec3/residual_echo_estimator.cc
    ../modules/audio_processing/aec3/reverb_decay_estimator.cc
    ../modules/audio_processing/aec3/reverb_frequency_response.cc
    ../modules/audio_processing/aec3/reverb_model.cc
    ../modules/audio_processing/aec3/reverb_model_estimator.cc
    ../modules/audio_processing/aec3/signal_dependent_erle_estimator.cc
    ../modules/audio_processing/aec3/spectrum_buffer.cc
    ../modules/audio_processing/aec3/stationarity_estimator.cc
    ../modules/audio_processing/aec3/subband_erle_estimator.cc
    ../modules/audio_processing/aec3/subband_nearend_detector.cc
    ../modules/audio_processing/aec3/subtractor.cc
    ../modules/audio_processing/aec3/subtractor_output.cc
    ../modules/audio_processing/aec3/subtractor_output_analyzer.cc
    ../modules/audio_processing/aec3/suppression_filter.cc
    ../modules/audio_processing/aec3/suppression_gain.cc
    ../modules/audio_processing/aec3/transparent_mode.cc
    ../modules/audio_processing/aec3/vector_math_avx2.cc
    ../modules/audio_processing/audio_buffer.cc
    ../modules/audio_processing/echo_detector/circular_buffer.cc
    ../modules/audio_processing/echo_detector/mean_variance_estimator.cc
    ../modules/audio_processing/echo_detector/moving_max.cc
    ../modules/audio_processing/echo_detector/normalized_covariance_estimator.cc
    ../modules/audio_processing/high_pass_filter.cc
    ../modules/audio_processing/include/audio_frame_proxies.cc
    ../modules/audio_processing/include/audio_processing.cc
    ../modules/audio_processing/include/audio_processing_statistics.cc
    ../modules/audio_processing/logging/apm_data_dumper.cc
    ../modules/audio_processing/residual_echo_detector.cc
    ../modules/audio_processing/rms_level.cc
    ../modules/audio_processing/splitting_filter.cc
    ../modules/audio_processing/three_band_filter_bank.cc
    ../modules/audio_processing/utility/cascaded_biquad_filter.cc
    ../modules/audio_processing/utility/delay_estimator.cc
    ../modules/audio_processing/utility/delay_estimator_wrapper.cc
    ../modules/audio_processing/utility/pffft_wrapper.cc
    ../rtc_base/checks.cc
    ../rtc_base/experiments/field_trial_parser.cc
    ../rtc_base/logging.cc
    ../rtc_base/memory/aligned_malloc.cc
    ../rtc_base/platform_thread.cc
    ../rtc_base/platform_thread_types.cc
    ../rtc_base/race_checker.cc
    ../rtc_base/strings/audio_format_to_string.cc
    ../rtc_base/strings/string_builder.cc
    ../rtc_base/strings/string_format.cc
    ../rtc_base/string_encode.cc
    ../rtc_base/string_utils.cc
    ../rtc_base/system/file_wrapper.cc
    ../rtc_base/system_time.cc
    ../rtc_base/time_utils.cc
    ../system_wrappers/source/cpu_features.cc
    ../system_wrappers/source/cpu_info.cc
    ../system_wrappers/source/field_trial.cc
    ../system_wrappers/source/metrics.cc
    ../third_party/pffft/src/pffft.c
    main.cpp
    wave_file.cpp
)

# 添加可执行文件
add_executable(AEC3Demo ${SOURCES})

# 链接库
target_link_libraries(AEC3Demo
    pthread
    m
)
