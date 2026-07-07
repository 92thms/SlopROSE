# LoginServer executable.
# Self-contained: no Common/ dependency at all on the current Windows build.

add_executable(LoginServer
  SHO_LS_LIB/CAS_GUMS.cpp
  SHO_LS_LIB/CLS_Account.cpp
  SHO_LS_LIB/CLS_Client.cpp
  SHO_LS_LIB/CLS_Server.cpp
  SHO_LS_LIB/CLS_SQLThread.cpp
  SHO_LS_LIB/SHO_LS_LIB.CPP

  # former SHO_LS_EXE (EXE+DLL+LIB collapsed into one executable, M0 finding)
  SHO_LS_EXE/main.cpp
)

target_include_directories(LoginServer PRIVATE
  SHO_LS_LIB
)

target_link_libraries(LoginServer PRIVATE LIB_Server LIB_Util)
