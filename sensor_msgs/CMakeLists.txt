# Copyright 2018 Real-Time Innovations 
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

list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/BatteryState.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/CameraInfo.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/ChannelFloat32.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/CompressedImage.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/FluidPressure.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Illuminance.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Image.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Imu.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/JointState.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Joy.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/JoyFeedback.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/JoyFeedbackArray.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/LaserEcho.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/LaserScan.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/MagneticField.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/MultiDOFJointState.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/MultiEchoLaserScan.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/NavSatFix.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/NavSatStatus.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/PointCloud.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/PointCloud2.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/PointField.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Range.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/RegionOfInterest.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/RelativeHumidity.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/Temperature.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/msg/TimeReference.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/srv/SetCameraInfo_Request.idl")
list(APPEND idl_files "${CMAKE_CURRENT_SOURCE_DIR}/srv/SetCameraInfo_Response.idl")

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

add_library( sensormsgslibrary OBJECT
    ${generated_file_list}
)

set_property(TARGET sensormsgslibrary PROPERTY 
    POSITION_INDEPENDENT_CODE ON
)

target_include_directories(sensormsgslibrary PRIVATE 
    ${CONNEXTDDS_INCLUDE_DIRS}
    ${top_level_binary_dir}
)

target_compile_definitions(sensormsgslibrary PRIVATE 
    ${CONNEXTDDS_COMPILE_DEFINITIONS}
)

add_dependencies(sensormsgslibrary 
    stdmsgslibrary 
    geometrymsgslibrary
)
