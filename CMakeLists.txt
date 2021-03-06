cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB cryptonote)

include_directories(${CMAKE_BINARY_DIR}
    src
    katonycoin/external
    katonycoin/include
    katonycoin/src)

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
configure_file("katonycoin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  katonycoin/external/miniupnpc/connecthostport.c
  katonycoin/external/miniupnpc/igd_desc_parse.c
  katonycoin/external/miniupnpc/minisoap.c
  katonycoin/external/miniupnpc/miniupnpc.c
  katonycoin/external/miniupnpc/miniwget.c
  katonycoin/external/miniupnpc/minixml.c
  katonycoin/external/miniupnpc/portlistingparse.c
  katonycoin/external/miniupnpc/receivedata.c
  katonycoin/external/miniupnpc/upnpcommands.c
  katonycoin/external/miniupnpc/upnpreplyparse.c
  katonycoin/src/Common/Base58.cpp
  katonycoin/src/Common/CommandLine.cpp
  katonycoin/src/Common/Util.cpp
  katonycoin/src/Common/StringTools.cpp
  katonycoin/src/Common/JsonValue.cpp
  katonycoin/src/Common/ConsoleTools.cpp
  katonycoin/src/Common/MemoryInputStream.cpp
  katonycoin/src/Common/PathTools.cpp
  katonycoin/src/Common/StdInputStream.cpp
  katonycoin/src/Common/StdOutputStream.cpp
  katonycoin/src/Common/StreamTools.cpp
  katonycoin/src/Common/StringOutputStream.cpp
  katonycoin/src/Common/StringView.cpp
  katonycoin/src/Common/VectorOutputStream.cpp
  katonycoin/src/crypto/blake256.c
  katonycoin/src/crypto/chacha8.c
  katonycoin/src/crypto/crypto-ops-data.c
  katonycoin/src/crypto/crypto-ops.c
  katonycoin/src/crypto/crypto.cpp
  katonycoin/src/crypto/groestl.c
  katonycoin/src/crypto/hash-extra-blake.c
  katonycoin/src/crypto/hash-extra-groestl.c
  katonycoin/src/crypto/hash-extra-jh.c
  katonycoin/src/crypto/hash-extra-skein.c
  katonycoin/src/crypto/hash.c
  katonycoin/src/crypto/jh.c
  katonycoin/src/crypto/keccak.c
  katonycoin/src/crypto/oaes_lib.c
  katonycoin/src/crypto/random.c
  katonycoin/src/crypto/skein.c
  katonycoin/src/crypto/slow-hash.c
  katonycoin/src/crypto/slow-hash.cpp
  katonycoin/src/crypto/tree-hash.c
  katonycoin/src/CryptoNoteCore/BlockchainIndices.cpp
  katonycoin/src/CryptoNoteCore/BlockchainMessages.cpp
  katonycoin/src/CryptoNoteCore/BlockIndex.cpp
  katonycoin/src/CryptoNoteCore/CoreConfig.cpp
  katonycoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  katonycoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  katonycoin/src/CryptoNoteCore/Currency.cpp
  katonycoin/src/CryptoNoteCore/MinerConfig.cpp
  katonycoin/src/CryptoNoteCore/Transaction.cpp
  katonycoin/src/CryptoNoteCore/Account.cpp
  katonycoin/src/CryptoNoteCore/Blockchain.cpp
  katonycoin/src/CryptoNoteCore/Checkpoints.cpp
  katonycoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  katonycoin/src/CryptoNoteCore/Core.cpp
  katonycoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  katonycoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  katonycoin/src/CryptoNoteCore/Difficulty.cpp
  katonycoin/src/CryptoNoteCore/IBlock.cpp
  katonycoin/src/CryptoNoteCore/Miner.cpp
  katonycoin/src/CryptoNoteCore/TransactionExtra.cpp
  katonycoin/src/CryptoNoteCore/TransactionPool.cpp
  katonycoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  katonycoin/src/CryptoNoteCore/TransactionUtils.cpp
  katonycoin/src/InProcessNode/InProcessNode.cpp
  katonycoin/src/InProcessNode/InProcessNodeErrors.cpp
  katonycoin/src/NodeRpcProxy/NodeErrors.cpp
  katonycoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  katonycoin/src/P2p/NetNodeConfig.cpp
  katonycoin/src/Serialization/BinaryInputStreamSerializer.cpp
  katonycoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  katonycoin/src/Serialization/JsonInputValueSerializer.cpp
  katonycoin/src/Serialization/JsonOutputStreamSerializer.cpp
  katonycoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  katonycoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  katonycoin/src/Serialization/SerializationOverloads.cpp
  katonycoin/src/Transfers/BlockchainSynchronizer.cpp
  katonycoin/src/Transfers/SynchronizationState.cpp
  katonycoin/src/Transfers/TransfersConsumer.cpp
  katonycoin/src/Transfers/TransfersContainer.cpp
  katonycoin/src/Transfers/TransfersSubscription.cpp
  katonycoin/src/Transfers/TransfersSynchronizer.cpp
  katonycoin/src/Wallet/LegacyKeysImporter.cpp
  katonycoin/src/Wallet/WalletAsyncContextCounter.cpp
  katonycoin/src/Wallet/WalletErrors.cpp
  katonycoin/src/WalletLegacy/KeysStorage.cpp
  katonycoin/src/WalletLegacy/WalletLegacy.cpp
  katonycoin/src/WalletLegacy/WalletHelper.cpp
  katonycoin/src/WalletLegacy/WalletLegacySerializer.cpp
  katonycoin/src/WalletLegacy/WalletLegacySerialization.cpp
  katonycoin/src/WalletLegacy/WalletTransactionSender.cpp
  katonycoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  katonycoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  katonycoin/src/System/ContextGroup.cpp
  katonycoin/src/System/Event.cpp
  katonycoin/src/System/EventLock.cpp
  katonycoin/src/System/InterruptedException.cpp
  katonycoin/src/System/Ipv4Address.cpp
  katonycoin/src/System/TcpStream.cpp
  katonycoin/src/HTTP/HttpRequest.cpp
  katonycoin/src/HTTP/HttpParser.cpp
  katonycoin/src/HTTP/HttpParserErrorCodes.cpp
  katonycoin/src/HTTP/HttpResponse.cpp
  katonycoin/src/Rpc/HttpClient.cpp
  katonycoin/src/Rpc/JsonRpc.cpp
  katonycoin/src/P2p/NetNode.cpp
  katonycoin/src/P2p/LevinProtocol.cpp
  katonycoin/src/P2p/NetNodeConfig.cpp
  katonycoin/src/P2p/PeerListManager.cpp
  katonycoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  katonycoin/src/Logging/ILogger.cpp
  katonycoin/src/Logging/LoggerGroup.cpp
  katonycoin/src/Logging/CommonLogger.cpp
  katonycoin/src/Logging/LoggerManager.cpp
  katonycoin/src/Logging/FileLogger.cpp
  katonycoin/src/Logging/StreamLogger.cpp
  katonycoin/src/Logging/ConsoleLogger.cpp
  katonycoin/src/Logging/LoggerMessage.cpp
  katonycoin/src/Logging/LoggerRef.cpp
  katonycoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  katonycoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  katonycoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
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
  include_directories(katonycoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} katonycoin/external/miniupnpc/minissdpc.c)
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

include_directories(katonycoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES katonycoin/src/Platform/${PLATFORM_DIR}/System/*)
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
