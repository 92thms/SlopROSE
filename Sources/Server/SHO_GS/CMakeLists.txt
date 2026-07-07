# GameServer executable.
#
# NOTE on Common/: GameServer uses its own local copy of several
# "Common" files (SHO_GS_LIB/Common/*), which has genuinely diverged
# from the shared Sources/Common/* copy used by WorldServer -
# CItem.h/.cpp and CInventory.h/.cpp differ structurally (different
# item bitfield layouts), not just cosmetically. Do NOT consolidate
# these into a single shared target; each server keeps the file
# variant it actually built against on Windows.

add_executable(GameServer
  # shared Common/ (identical across servers, safe to take from top-level)
  ../../Common/CAssertNew.cpp
  ../../Common/MiniDumper.cpp

  # SHO_GS_LIB
  SHO_GS_LIB/Ai_lib/AI_Action.cpp
  SHO_GS_LIB/Ai_lib/AI_Condition.cpp
  SHO_GS_LIB/Ai_lib/CAI_FILE.CPP
  SHO_GS_LIB/AlphabetCvt/AlphabetCvt.cpp
  SHO_GS_LIB/CCharDATA.cpp
  SHO_GS_LIB/CGameOBJ.cpp
  SHO_GS_LIB/CheatCMD.cpp
  SHO_GS_LIB/CIngSTATUS.cpp
  SHO_GS_LIB/CObjAVT.cpp
  SHO_GS_LIB/CObjCHAR.cpp
  SHO_GS_LIB/CObjEVENT.cpp
  SHO_GS_LIB/CObjITEM.cpp
  SHO_GS_LIB/CObjNPC.cpp
  SHO_GS_LIB/Common/Calculation.cpp
  SHO_GS_LIB/Common/CEconomy.cpp
  SHO_GS_LIB/Common/CHotICON.cpp
  SHO_GS_LIB/Common/CInventory.cpp
  SHO_GS_LIB/Common/CItem.cpp
  SHO_GS_LIB/Common/CObjAI.cpp
  SHO_GS_LIB/Common/CQuest.cpp
  SHO_GS_LIB/Common/CRegenAREA.cpp
  SHO_GS_LIB/Common/CUserDATA.cpp
  SHO_GS_LIB/Common/IO_AI.CPP
  SHO_GS_LIB/Common/IO_Motion.cpp
  SHO_GS_LIB/Common/IO_PAT.cpp
  SHO_GS_LIB/Common/IO_Quest.cpp
  SHO_GS_LIB/Common/IO_Skill.cpp
  SHO_GS_LIB/Common/IO_STB.cpp
  SHO_GS_LIB/ETC_Math.cpp
  SHO_GS_LIB/GS_ListUSER.cpp
  SHO_GS_LIB/GS_ObjPOOL.cpp
  SHO_GS_LIB/GS_PARTY.CPP
  SHO_GS_LIB/GS_SocketASV.cpp
  SHO_GS_LIB/GS_SocketLSV.cpp
  SHO_GS_LIB/GS_ThreadLOG.cpp
  SHO_GS_LIB/GS_ThreadMALL.cpp
  SHO_GS_LIB/GS_ThreadSQL.cpp
  SHO_GS_LIB/GS_ThreadZONE.cpp
  SHO_GS_LIB/GS_USER.cpp
  SHO_GS_LIB/LIB_gsMAIN.cpp
  SHO_GS_LIB/md5.cpp
  SHO_GS_LIB/OBJECT.cpp
  SHO_GS_LIB/srv_COMMON/CChatROOM.cpp
  SHO_GS_LIB/srv_COMMON/CThreadGUILD.CPP
  SHO_GS_LIB/srv_COMMON/CThreadLOG.cpp
  SHO_GS_LIB/srv_COMMON/CThreadMSGR.cpp
  SHO_GS_LIB/ZoneFILE.cpp
  SHO_GS_LIB/ZoneLIST.cpp
  SHO_GS_LIB/ZoneSECTOR.cpp

  # former SHO_GS_EXE (EXE+DLL+LIB collapsed into one executable, M0 finding)
  SHO_GS_EXE/main.cpp
)

target_include_directories(GameServer PRIVATE
  SHO_GS_LIB
  SHO_GS_LIB/Common
  SHO_GS_LIB/srv_COMMON
  SHO_GS_LIB/Ai_lib
  ${CMAKE_SOURCE_DIR}/Sources/Common
)

target_link_libraries(GameServer PRIVATE LIB_Server LIB_Util)
