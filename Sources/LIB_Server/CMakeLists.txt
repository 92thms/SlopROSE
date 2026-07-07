add_library(LIB_Server STATIC
  blockLIST.cpp
  CAcceptTHREAD.cpp
  CIocpTHREAD.cpp
  classODBC.cpp
  classPACKET.cpp
  classSQL.cpp
  CSqlTHREAD.cpp
  CurlTHREAD.cpp
  iocpSOCKET.cpp
  iocpSocketSERVER.cpp
  ioDataPOOL.cpp
  ipLIST.cpp
  IP_Addr.cpp
)

target_include_directories(LIB_Server PUBLIC
  ${CMAKE_CURRENT_SOURCE_DIR}
)

target_link_libraries(LIB_Server PUBLIC LIB_Util)
