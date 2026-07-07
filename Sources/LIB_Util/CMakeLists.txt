add_library(LIB_Util STATIC
  CBITArray.cpp
  CClientSOCKET.cpp
  cdxHPC.cpp
  classCRC.cpp
  classFile.cpp
  classHASH.cpp
  classHTTP.cpp
  classIME.cpp
  classLOG.cpp
  classMD5.cpp
  classSTB.cpp
  classSTR.cpp
  classSYNCOBJ.cpp
  classSYS.cpp
  classTHREAD.cpp
  classTIME.cpp
  classTRACE.cpp
  classUTIL.cpp
  CPacketCODEC.cpp
  CProcess.cpp
  CRandom.cpp
  CRawSOCKET.cpp
  CshoSOCKET.cpp
  CSocket.cpp
  CSocketWND.cpp
  LIB_IJL.cpp
  PacketHEADER.cpp
)

target_include_directories(LIB_Util PUBLIC
  ${CMAKE_CURRENT_SOURCE_DIR}
  # CBITArray.h is a plain, Windows-independent utility header that
  # happens to live under Client/Util/ but is needed by server-side
  # code (via Common/CQuest.h). Candidate for relocation in M6; kept
  # in place for now to avoid an unrelated churn during M1.
  ${CMAKE_SOURCE_DIR}/Sources/Client/Util
)
