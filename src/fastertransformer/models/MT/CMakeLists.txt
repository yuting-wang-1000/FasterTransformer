cmake_minimum_required(VERSION 3.8)

add_library(MTDecoder STATIC Decoder.cc)
set_property(TARGET MTDecoder PROPERTY POSITION_INDEPENDENT_CODE  ON)
set_property(TARGET MTDecoder PROPERTY CUDA_RESOLVE_DEVICE_SYMBOLS  ON)
target_link_libraries(MTDecoder PUBLIC -lcudart cublasMMWrapper DecoderSelfAttentionLayer
                    DecoderCrossAttentionLayer FfnLayer layernorm_kernels add_residual_kernels tensor cuda_utils logger)


add_library(MTDecoding STATIC Decoding.cc)
set_property(TARGET MTDecoding PROPERTY POSITION_INDEPENDENT_CODE  ON)
set_property(TARGET MTDecoding PROPERTY CUDA_RESOLVE_DEVICE_SYMBOLS  ON)
target_link_libraries(MTDecoding PUBLIC -lcublas cublasMMWrapper -lcudart -lcurand MTDecoder decoding_kernels
                        BeamSearchLayer DynamicDecodeLayer tensor cuda_utils logger)
                        
add_executable(mt_example mt_example.cc)
target_link_libraries(mt_example PUBLIC -lcublas -lcublasLt -lcudart nvtx_utils MTDecoder MTDecoding)

                        
