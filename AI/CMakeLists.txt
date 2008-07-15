include_directories(${CMAKE_SOURCE_DIR}/rts ${CMAKE_SOURCE_DIR}/rts/ExternalAI ${CMAKE_SOURCE_DIR}/rts/System)

add_definitions(-fPIC)
aux_source_directory(${CMAKE_SOURCE_DIR}/rts/System/creg creg)
list (APPEND creg ${CMAKE_SOURCE_DIR}/rts/System/float3)

### Global AIs
aux_source_directory(Global/KAI-0.2 kai02)
add_library(KAI-0.2 SHARED ${kai02} ${creg})

aux_source_directory(Global/KAIK-0.13 kai013)
add_library(KAIK-0.13 SHARED ${kai013} ${creg})

aux_source_directory(Global/RAI-0.553 rai)
add_library(RAI_0.553 SHARED ${rai} Global/RAI-0.553/KrogsMetalClass/MetalMap)

aux_source_directory(Global/AAI aai)
add_library(AAI SHARED ${aai})

if (UNIX)
	# doesn't build in win32currently
	aux_source_directory(Global/JCAI jcai)
	add_library(JCAI SHARED ${jcai})
endif (UNIX)

aux_source_directory(Global/NTai/AI/NTai/Units ntai)
aux_source_directory(Global/NTai/AI/NTai/Units/Behaviours ntai)
aux_source_directory(Global/NTai/AI/NTai/Tasks ntai)
aux_source_directory(Global/NTai/AI/NTai/Helpers ntai)
aux_source_directory(Global/NTai/AI/NTai/Helpers/grid ntai)
aux_source_directory(Global/NTai/AI/NTai/Helpers/Terrain ntai)
aux_source_directory(Global/NTai/AI/NTai/Helpers/Units ntai)
aux_source_directory(Global/NTai/AI/NTai/Engine ntai)
aux_source_directory(Global/NTai/AI/NTai/Core ntai)
aux_source_directory(Global/NTai/AI/NTai/Agents ntai)
add_library(NTai SHARED ${ntai})
target_link_libraries(NTai boost_thread-mt)

aux_source_directory(Global/TestGlobalAI testai)
add_library(TestGlobalAI SHARED ${testai})


if (UNIX)
install (TARGETS KAIK-0.13 KAI-0.2 RAI_0.553 AAI JCAI NTai TestGlobalAI DESTINATION spring/AI/Bot-libs)
else (UNIX)
install (TARGETS KAIK-0.13 KAI-0.2 RAI_0.553 AAI NTai TestGlobalAI DESTINATION spring/AI/Bot-libs)
endif (UNIX)

### Group AIs
aux_source_directory(Group/CentralBuildAI central)
add_library(CentralBuildAI SHARED ${central})

aux_source_directory(Group/EconomyAI economy)
add_library(EconomyAI SHARED ${economy} ${creg})

aux_source_directory(Group/MetalMakerAI metal)
add_library(MetalMakerAI SHARED ${metal})

aux_source_directory(Group/MexUpgraderAI mex)
add_library(MexUpgraderAI SHARED ${mex} ${creg})

aux_source_directory(Group/RadarAI radar)
add_library(RadarAI SHARED ${radar})

aux_source_directory(Group/ReportIdleAI idle)
add_library(ReportIdleAI SHARED ${idle})

aux_source_directory(Group/SimpleFormationAI formation)
add_library(SimpleFormationAI SHARED ${formation})


install (TARGETS CentralBuildAI EconomyAI MetalMakerAI MexUpgraderAI RadarAI ReportIdleAI SimpleFormationAI DESTINATION spring/AI/Helper-libs)