# ~~~
# Copyright 2018 Google LLC
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
# ~~~

set(storage_client_integration_tests
    bucket_integration_test.cc
    curl_download_request_integration_test.cc
    curl_request_integration_test.cc
    object_write_streambuf_integration_test.cc
    curl_resumable_upload_session_integration_test.cc
    curl_sign_blob_integration_test.cc
    error_injection_integration_test.cc
    key_file_integration_test.cc
    object_checksum_integration_test.cc
    object_file_integration_test.cc
    object_hash_integration_test.cc
    object_insert_integration_test.cc
    object_integration_test.cc
    object_media_integration_test.cc
    object_resumable_write_integration_test.cc
    object_rewrite_integration_test.cc
    service_account_integration_test.cc
    signed_url_conformance_test.cc
    signed_url_integration_test.cc
    slow_reader_integration_test.cc
    storage_include_test.cc
    thread_integration_test.cc)

foreach (fname ${storage_client_integration_tests})
    string(REPLACE "/"
                   "_"
                   target
                   ${fname})
    string(REPLACE ".cc"
                   ""
                   target
                   ${target})
    add_executable(${target} ${fname})
    target_link_libraries(${target}
                          PRIVATE storage_client
                                  storage_client_testing
                                  google_cloud_cpp_testing
                                  google_cloud_cpp_common
                                  GTest::gmock_main
                                  GTest::gmock
                                  GTest::gtest
                                  CURL::libcurl
                                  Threads::Threads
                                  nlohmann_json
                                  storage_common_options)
endforeach ()

target_link_libraries(error_injection_integration_test PRIVATE ${CMAKE_DL_LIBS})

include(CreateBazelConfig)
export_list_to_bazel("storage_client_integration_tests.bzl"
                     "storage_client_integration_tests")

add_subdirectory(parallel_upload_regression)
add_subdirectory(read_object_stall_regression)
add_subdirectory(write_deadlock_regression)
