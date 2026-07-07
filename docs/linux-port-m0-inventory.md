# Linux Port — M0 Build-Graph Inventory

Erzeugt aus den tatsächlichen ClCompile/ClInclude-Einträgen der vcxproj-Dateien (nicht aus Verzeichnis-Scans).

## LIB_Util
```
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
```

## LIB_Server
```
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
```

## SHO_GS_LIB
```
..\..\..\Common\CAssertNew.cpp
..\..\..\Common\MiniDumper.cpp
Ai_lib\AI_Action.cpp
Ai_lib\AI_Condition.cpp
Ai_lib\Cai_file.cpp
AlphabetCvt\AlphabetCvt.cpp
CCharDATA.cpp
CGameOBJ.cpp
CheatCMD.cpp
CIngSTATUS.cpp
CObjAVT.cpp
CObjCHAR.cpp
CObjEVENT.cpp
CObjITEM.cpp
CObjNPC.cpp
Common\Calculation.cpp
Common\CEconomy.cpp
Common\CHotICON.cpp
Common\CInventory.cpp
Common\CItem.cpp
Common\CObjAI.cpp
Common\CQuest.cpp
Common\CRegenAREA.cpp
Common\CUserDATA.cpp
Common\Io_ai.cpp
Common\IO_Motion.cpp
Common\IO_PAT.cpp
Common\IO_Quest.cpp
Common\IO_Skill.cpp
Common\IO_STB.cpp
ETC_Math.cpp
GS_ListUSER.cpp
GS_ObjPOOL.cpp
GS_PARTY.CPP
GS_SocketASV.cpp
GS_SocketLSV.cpp
GS_ThreadLOG.cpp
GS_ThreadMALL.cpp
GS_ThreadSQL.cpp
GS_ThreadZONE.cpp
GS_USER.cpp
LIB_gsMAIN.cpp
md5.cpp
OBJECT.cpp
srv_COMMON\CChatROOM.cpp
srv_COMMON\CThreadGUILD.CPP
srv_COMMON\CThreadLOG.cpp
srv_COMMON\CThreadMSGR.cpp
ZoneFILE.cpp
ZoneLIST.cpp
ZoneSECTOR.cpp
```

## SHO_GS_EXE
```
main.cpp
```

## SHO_LS_LIB
```
CAS_GUMS.cpp
CLS_Account.cpp
CLS_Client.cpp
CLS_Server.cpp
CLS_SQLThread.cpp
SHO_LS_LIB.CPP
```

## SHO_LS_EXE
```
main.cpp
```

## SHO_WS_LIB
```
..\..\..\Common\CChatROOM.cpp
..\..\..\Common\CDB_Socket.cpp
..\..\..\Common\CInventory.cpp
..\..\..\Common\CItem.cpp
..\..\..\Common\CThreadGUILD.CPP
..\..\..\Common\CThreadLOG.cpp
..\..\..\Common\CThreadMSGR.cpp
..\..\..\Common\ETC_Math.cpp
..\..\..\Common\IO_PAT.cpp
..\..\..\Common\IO_Skill.cpp
..\..\..\Common\IO_STB.cpp
..\..\..\LIB_Util\DebugReport.cpp
CWS_Account.cpp
CWS_Client.cpp
CWS_Party.cpp
CWS_Server.cpp
SHO_WS_LIB.CPP
StdAfx.cpp
WS_SocketLSV.cpp
WS_ThreadSQL.cpp
WS_ZoneLIST.cpp
```

## SHO_WS_EXE
```
main.cpp
```

Anmerkung: `SHO_GS_DLL`, `SHO_LS_DLL`, `SHO_WS_DLL` sind hier bewusst ausgelassen — jeweils nur 1-2 ClCompile-Einträge (`StdAfx.cpp` + ein `__stdcall`-Export-Shim über die eigentliche `CLIB_*SRV`-Singleton-Klasse). Der `LoadLibrary`/`GetProcAddress`-Ladepfad ist im Code bereits auskommentiert, `DllMain` enthält nur einen `Sleep(300)`-Hack. Bestätigt: EXE/DLL/LIB-Dreiteilung kann pro Server zu einer nativen Executable kollabiert werden (M1).

## Ergebnisse M0

**1. Toter VC6-IOCP-Baum:** `LIB_Server/IOCP/IocpQueue*` und `LIB_Server/IOCP/IocpServerVC6/*` sind in keiner `.vcxproj` im gesamten Repo referenziert (nur `classIOCP.h`, eine andere Datei auf oberster LIB_Server-Ebene, wird gebraucht). → sicher löschbar in M6.

**2. Doppelte `CDB_Socket`:** Nur `Common/CDB_Socket.cpp/h` ist lebender Code — kompiliert einzig von `SHO_WS_LIB` (WorldServer). Die zweite Kopie `Server/SHO_GS/SHO_GS_LIB/srv_COMMON/CDB_Socket.cpp/h` ist zusammen mit ihrem einzigen Verwender `GS_SocketLOG.cpp/h` **komplett tot**:
   - Weder `CDB_Socket.cpp` (srv_COMMON) noch `GS_SocketLOG.cpp` erscheinen in `SHO_GS_LIB.vcxproj`.
   - Der einzige potenzielle Aufrufer, `GS_ThreadLOG.cpp`, hat das `#include "GS_SocketLOG.h"` bereits auskommentiert (Zeile 3).
   - Bemerkenswert am Rande: aktuell nutzt **nur WorldServer** die `CDB_Socket`-Klasse produktiv — GameServer und LoginServer verwenden sie gar nicht. Ändert nichts an der M5-Reihenfolge, ist aber gut zu wissen für die DB-Migration (M4).
   - → `srv_COMMON/CDB_Socket.*` und `GS_SocketLOG.*` werden beim Port nicht mit übernommen (Streichung in M6, nicht in die Datei-Liste für M1 aufnehmen).

**Damit ist die Datei-Liste oben die verbindliche Grundlage für die CMake-Targets in M1** (LIB_Util, LIB_Server, dann je Server: `SHO_xx_LIB`-Liste + `SHO_xx_EXE/main.cpp`, ohne die drei DLL-Shims und ohne die beiden bestätigt toten Dateipaare).

## Weitere Funde beim Aufsetzen der CMake-Struktur (M1)

**3. Zwei unterschiedliche `Common`-Verzeichnisse, teils inhaltlich divergent:** GameServer kompiliert eine eigene lokale Kopie mehrerer „Common"-Dateien (`Server/SHO_GS/SHO_GS_LIB/Common/`), WorldServer die geteilte (`Sources/Common/`). `IO_PAT.*`, `IO_Skill.*`, `IO_STB.*` sind zwischen beiden Kopien identisch, aber **`CItem.h`/`.cpp` und `CInventory.h`/`.cpp` weichen strukturell voneinander ab** (z.B. unterschiedliche Bitfeld-Breiten in `tagPartITEM`, GS nutzt 26 Bit für `m_nItemNo`, die geteilte Version nur 10 Bit). Das ist kein Redundanz-Versehen, sondern ein echter Gameplay-Fork zwischen den Servern. → In der CMake-Struktur bekommt jeder Server exakt die Datei-Variante, die er auf Windows tatsächlich gebaut hat; es gibt **kein** gemeinsames „Common"-Target für diese Dateien. `ETC_Math.cpp` weicht nur um eine Leerzeile ab (unkritisch).

**4. `CBITArray.h` fehlt am erwarteten Ort:** `LIB_Util/CBITArray.cpp` inkludiert `CBITArray.h`, die Datei existiert aber nirgends in `LIB_Util` — sie liegt (Windows-Include-Pfad `$(SolutionDir)Client\Util`) unter `Sources/Client/Util/CBITArray.h`, einem eigentlich Client-only-Verzeichnis. Der Header selbst ist plain C++ ohne Windows-Abhängigkeit und wird auch von serverseitigem Code gebraucht (`CQuest.h`). Für M1 pragmatisch gelöst: `Sources/Client/Util` als zusätzlicher Include-Pfad für `LIB_Util` (PUBLIC, vererbt sich an alle Server). Kandidat für M6: den Header an einen sauberen gemeinsamen Ort verschieben, statt dauerhaft in den Client-Baum zu greifen.

**5. Case-Sensitivity-Bug real vorhanden:** `LIB_Util/cdxHPC.cpp` inkludierte `"cdxhpc.h"` (klein geschrieben), die Datei heißt aber `CDXHPC.H`. Unter Windows (case-insensitives Dateisystem) unsichtbar, unter Linux ein harter Fehler. Behoben durch Anpassung des Include auf den echten Dateinamen.

**Validierung:** `cmake` (Configure) läuft fehlerfrei durch (alle Quelldatei-Pfade lösen sich korrekt auf). Ein `cmake --build . -k` über den kompletten Graphen liefert **ausschließlich** noch offene Fehler der Form `windows.h`/`winsock.h`/`winsock2.h`/`crtdbg.h` nicht gefunden — also exakt die Klasse von Fehlern, die M2 beheben soll. Keine weiteren Pfad-, Case- oder Datei-Probleme mehr aufgetaucht.


