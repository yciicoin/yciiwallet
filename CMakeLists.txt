cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB cryptonote)

include_directories(${CMAKE_BINARY_DIR}
    src
    ycii/external
    ycii/include
    ycii/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("ycii/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  ycii/external/miniupnpc/connecthostport.c
  ycii/external/miniupnpc/igd_desc_parse.c
  ycii/external/miniupnpc/minisoap.c
  ycii/external/miniupnpc/miniupnpc.c
  ycii/external/miniupnpc/miniwget.c
  ycii/external/miniupnpc/minixml.c
  ycii/external/miniupnpc/portlistingparse.c
  ycii/external/miniupnpc/receivedata.c
  ycii/external/miniupnpc/upnpcommands.c
  ycii/external/miniupnpc/upnpreplyparse.c
  ycii/src/Common/Base58.cpp
  ycii/src/Common/CommandLine.cpp
  ycii/src/Common/Util.cpp
  ycii/src/Common/StringTools.cpp
  ycii/src/Common/JsonValue.cpp
  ycii/src/Common/ConsoleTools.cpp
  ycii/src/Common/MemoryInputStream.cpp
  ycii/src/Common/PathTools.cpp
  ycii/src/Common/StdInputStream.cpp
  ycii/src/Common/StdOutputStream.cpp
  ycii/src/Common/StreamTools.cpp
  ycii/src/Common/StringOutputStream.cpp
  ycii/src/Common/StringView.cpp
  ycii/src/Common/VectorOutputStream.cpp
  ycii/src/crypto/blake256.c
  ycii/src/crypto/chacha8.c
  ycii/src/crypto/crypto-ops-data.c
  ycii/src/crypto/crypto-ops.c
  ycii/src/crypto/crypto.cpp
  ycii/src/crypto/groestl.c
  ycii/src/crypto/hash-extra-blake.c
  ycii/src/crypto/hash-extra-groestl.c
  ycii/src/crypto/hash-extra-jh.c
  ycii/src/crypto/hash-extra-skein.c
  ycii/src/crypto/hash.c
  ycii/src/crypto/jh.c
  ycii/src/crypto/keccak.c
  ycii/src/crypto/oaes_lib.c
  ycii/src/crypto/random.c
  ycii/src/crypto/skein.c
  ycii/src/crypto/slow-hash.c
  ycii/src/crypto/slow-hash.cpp
  ycii/src/crypto/tree-hash.c
  ycii/src/CryptoNoteCore/BlockchainIndices.cpp
  ycii/src/CryptoNoteCore/BlockchainMessages.cpp
  ycii/src/CryptoNoteCore/BlockIndex.cpp
  ycii/src/CryptoNoteCore/CoreConfig.cpp
  ycii/src/CryptoNoteCore/CryptoNoteBasic.cpp
  ycii/src/CryptoNoteCore/CryptoNoteTools.cpp
  ycii/src/CryptoNoteCore/Currency.cpp
  ycii/src/CryptoNoteCore/MinerConfig.cpp
  ycii/src/CryptoNoteCore/Transaction.cpp
  ycii/src/CryptoNoteCore/Account.cpp
  ycii/src/CryptoNoteCore/Blockchain.cpp
  ycii/src/CryptoNoteCore/Checkpoints.cpp
  ycii/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  ycii/src/CryptoNoteCore/Core.cpp
  ycii/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  ycii/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  ycii/src/CryptoNoteCore/Difficulty.cpp
  ycii/src/CryptoNoteCore/IBlock.cpp
  ycii/src/CryptoNoteCore/Miner.cpp
  ycii/src/CryptoNoteCore/TransactionExtra.cpp
  ycii/src/CryptoNoteCore/TransactionPool.cpp
  ycii/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  ycii/src/CryptoNoteCore/TransactionUtils.cpp
  ycii/src/InProcessNode/InProcessNode.cpp
  ycii/src/InProcessNode/InProcessNodeErrors.cpp
  ycii/src/NodeRpcProxy/NodeErrors.cpp
  ycii/src/NodeRpcProxy/NodeRpcProxy.cpp
  ycii/src/P2p/NetNodeConfig.cpp
  ycii/src/Serialization/BinaryInputStreamSerializer.cpp
  ycii/src/Serialization/BinaryOutputStreamSerializer.cpp
  ycii/src/Serialization/JsonInputValueSerializer.cpp
  ycii/src/Serialization/JsonOutputStreamSerializer.cpp
  ycii/src/Serialization/KVBinaryInputStreamSerializer.cpp
  ycii/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  ycii/src/Serialization/SerializationOverloads.cpp
  ycii/src/Transfers/BlockchainSynchronizer.cpp
  ycii/src/Transfers/SynchronizationState.cpp
  ycii/src/Transfers/TransfersConsumer.cpp
  ycii/src/Transfers/TransfersContainer.cpp
  ycii/src/Transfers/TransfersSubscription.cpp
  ycii/src/Transfers/TransfersSynchronizer.cpp
  ycii/src/Wallet/LegacyKeysImporter.cpp
  ycii/src/Wallet/WalletAsyncContextCounter.cpp
  ycii/src/Wallet/WalletErrors.cpp
  ycii/src/WalletLegacy/KeysStorage.cpp
  ycii/src/WalletLegacy/WalletLegacy.cpp
  ycii/src/WalletLegacy/WalletHelper.cpp
  ycii/src/WalletLegacy/WalletLegacySerializer.cpp
  ycii/src/WalletLegacy/WalletLegacySerialization.cpp
  ycii/src/WalletLegacy/WalletTransactionSender.cpp
  ycii/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  ycii/src/WalletLegacy/WalletUserTransactionsCache.cpp
  ycii/src/System/ContextGroup.cpp
  ycii/src/System/Event.cpp
  ycii/src/System/EventLock.cpp
  ycii/src/System/InterruptedException.cpp
  ycii/src/System/Ipv4Address.cpp
  ycii/src/System/TcpStream.cpp
  ycii/src/HTTP/HttpRequest.cpp
  ycii/src/HTTP/HttpParser.cpp
  ycii/src/HTTP/HttpParserErrorCodes.cpp
  ycii/src/HTTP/HttpResponse.cpp
  ycii/src/Rpc/HttpClient.cpp
  ycii/src/Rpc/JsonRpc.cpp
  ycii/src/P2p/NetNode.cpp
  ycii/src/P2p/LevinProtocol.cpp
  ycii/src/P2p/NetNodeConfig.cpp
  ycii/src/P2p/PeerListManager.cpp
  ycii/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  ycii/src/Logging/ILogger.cpp
  ycii/src/Logging/LoggerGroup.cpp
  ycii/src/Logging/CommonLogger.cpp
  ycii/src/Logging/LoggerManager.cpp
  ycii/src/Logging/FileLogger.cpp
  ycii/src/Logging/StreamLogger.cpp
  ycii/src/Logging/ConsoleLogger.cpp
  ycii/src/Logging/LoggerMessage.cpp
  ycii/src/Logging/LoggerRef.cpp
  ycii/src/BlockchainExplorer/BlockchainExplorer.cpp
  ycii/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  ycii/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(ycii/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ycii/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(ycii/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES ycii/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
