# WorldServer executable.
#
# Uses the shared top-level Common/ copies of CItem/CInventory/etc.
# (not the GameServer-local fork under SHO_GS_LIB/Common - see the
# comment in Server/SHO_GS/CMakeLists.txt for why those two must not
# be consolidated).

add_executable(WorldServer
  ../../Common/CChatROOM.cpp
  ../../Common/CDB_Socket.cpp
  ../../Common/CInventory.cpp
  ../../Common/CItem.cpp
  ../../Common/CThreadGUILD.CPP
  ../../Common/CThreadLOG.cpp
  ../../Common/CThreadMSGR.cpp
  ../../Common/ETC_Math.cpp
  ../../Common/IO_PAT.cpp
  ../../Common/IO_Skill.cpp
  ../../Common/IO_STB.cpp
  ../../LIB_Util/DebugReport.cpp

  SHO_WS_LIB/CWS_Account.cpp
  SHO_WS_LIB/CWS_Client.cpp
  SHO_WS_LIB/CWS_Party.cpp
  SHO_WS_LIB/CWS_Server.cpp
  SHO_WS_LIB/SHO_WS_LIB.CPP
  SHO_WS_LIB/StdAfx.cpp
  SHO_WS_LIB/WS_SocketLSV.cpp
  SHO_WS_LIB/WS_ThreadSQL.cpp
  SHO_WS_LIB/WS_ZoneLIST.cpp

  # former SHO_WS_EXE (EXE+DLL+LIB collapsed into one executable, M0 finding)
  SHO_WS_EXE/main.cpp
)

target_include_directories(WorldServer PRIVATE
  SHO_WS_LIB
  ${CMAKE_SOURCE_DIR}/Sources/Common
)

target_link_libraries(WorldServer PRIVATE LIB_Server LIB_Util)
