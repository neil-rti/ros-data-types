# Copyright 2020 Real-Time Innovations 
# 
# Licensed under the Apache License, Version 2.0 (the "License"); 
# you may not use this file except in compliance with the License. 
# You may obtain a copy of the License at 
# 
#     http://www.apache.org/licenses/LICENSE-2.0 
# 
# Unless required by applicable law or agreed to in writing, software 
# distributed under the License is distributed on an "AS IS" BASIS, 
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. 
# See the License for the specific language governing permissions and 
# limitations under the License. 

include(ConnextDdsRosDdsTypes)

list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/action/Fibonacci_Feedback.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/action/Fibonacci_FeedbackMessage.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/action/Fibonacci_GetResult_Request.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/action/Fibonacci_GetResult_Response.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/action/Fibonacci_Goal.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/action/Fibonacci_Result.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/action/Fibonacci_SendGoal_Request.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/action/Fibonacci_SendGoal_Response.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Bool.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Byte.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/ByteMultiArray.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Char.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Empty.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Float32.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Float32MultiArray.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Float64.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Float64MultiArray.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Int16.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Int16MultiArray.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Int32.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Int32MultiArray.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Int64.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Int64MultiArray.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Int8.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Int8MultiArray.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/MultiArrayDimension.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/MultiArrayLayout.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/String.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/UInt16.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/UInt16MultiArray.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/UInt32.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/UInt32MultiArray.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/UInt64.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/UInt64MultiArray.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/UInt8.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/UInt8MultiArray.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/WString.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/srv/AddTwoInts_Request.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/srv/AddTwoInts_Response.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/srv/SetBool_Request.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/srv/SetBool_Response.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/srv/Trigger_Request.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/srv/Trigger_Response.idl")

# for unbounded strings & sequences use -DUNBOUNDED_ALL on CMake cmdline
if(UNBOUNDED_ALL)
    set(extra_params UNBOUNDED)
endif()

connextdds_generate_ros_dds_types(
    LANG ${LANG}
    OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}"
    IDL_FILES ${idl_files}
    INCLUDE_DIRS ${top_level_source_dir}
    ${extra_params}
)
add_library( exampleinterfaceslibrary OBJECT
    ${generated_file_list}
)

set_property(TARGET exampleinterfaceslibrary PROPERTY 
    POSITION_INDEPENDENT_CODE ON
)

target_include_directories(exampleinterfaceslibrary PRIVATE 
    ${CONNEXTDDS_INCLUDE_DIRS}
    ${top_level_binary_dir}
)

target_compile_definitions(exampleinterfaceslibrary PRIVATE 
    ${CONNEXTDDS_COMPILE_DEFINITIONS}
)

add_dependencies(exampleinterfaceslibrary 
    stdmsgslibrary 
    uniqueidentifiermsgslibrary 
    builtininterfaceslibrary
)
