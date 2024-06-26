# Target for Tools
add_library( _nova_tools
  src/Grids/Grid.cpp
  src/Grids/Grid_Iterator_Cell.cpp
  src/Grids/Grid_Iterator_Face.cpp
  src/Krylov_Solvers/Conjugate_Gradient.cpp
  src/Krylov_Solvers/Krylov_System_Base.cpp
  src/Log/Debug_Utilities.cpp
  src/Log/Log.cpp
  src/Log/Log_Entry.cpp
  src/Parsing/Parse_Args.cpp
  src/Random_Numbers/MT19937.cpp
  src/Utilities/Driver.cpp
  src/Utilities/Example.cpp
  src/Utilities/Exceptions.cpp
  src/Utilities/File_Utilities.cpp
  src/Utilities/Pthread_Queue.cpp
  src/Utilities/Timer.cpp
  src/Vectors/Vector_2D.cpp
  src/Vectors/Vector_3D.cpp
  )
if(!APPLE)
add_library( _nova_tools
  src/Images/PNG_File.cpp
)
endif()
find_package(Boost 1.54 REQUIRED COMPONENTS filesystem)

if(USE_DOUBLES)
  target_compile_definitions(_nova_tools PUBLIC COMPILE_WITH_DOUBLE_SUPPORT=1)
endif()

target_include_directories( _nova_tools PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
  PRIVATE src )

target_compile_features( _nova_tools
  PUBLIC
  cxx_variadic_templates
  cxx_nullptr
  )

target_link_libraries( _nova_tools
  PUBLIC
  ${Boost_LIBRARIES}
  )

if(!APPLE)
target_link_libraries( _nova_tools
  PUBLIC
  png
)
endif()

add_library( nova::tools ALIAS _nova_tools )
