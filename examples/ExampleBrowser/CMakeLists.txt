
INCLUDE_DIRECTORIES(
	.
	${BULLET_PHYSICS_SOURCE_DIR}/src
	${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs
)

FILE(GLOB GwenGUISupport_SRCS  "GwenGUISupport/*" )
FILE(GLOB GwenGUISupport_HDRS  "GwenGUISupport/*" )


IF (WIN32)
        INCLUDE_DIRECTORIES(
                ${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs/Glew
        )
				ADD_DEFINITIONS(-DGLEW_STATIC)
ELSE(WIN32)
        IF(APPLE)
                find_library(COCOA NAMES Cocoa)
  			ELSE(APPLE)
                ADD_DEFINITIONS("-DGLEW_INIT_OPENGL11_FUNCTIONS=1")
                ADD_DEFINITIONS("-DGLEW_STATIC")
                ADD_DEFINITIONS("-DGLEW_DYNAMIC_LOAD_ALL_GLX_FUNCTIONS=1")
                INCLUDE_DIRECTORIES( ${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs/Glew )
     		ENDIF(APPLE)
ENDIF(WIN32)


ADD_LIBRARY(BulletExampleBrowserLib
	OpenGLExampleBrowser.cpp
	OpenGLGuiHelper.cpp
	GL_ShapeDrawer.cpp
	CollisionShape2TriangleMesh.cpp
	CollisionShape2TriangleMesh.h
	../Utils/b3Clock.cpp
	../Utils/b3Clock.h
	../Utils/b3ResourcePath.cpp
	../Utils/b3ResourcePath.h
	${GwenGUISupport_SRCS}
	${GwenGUISupport_HDRS}
	
)

SET_TARGET_PROPERTIES(BulletExampleBrowserLib PROPERTIES VERSION ${BULLET_VERSION})
SET_TARGET_PROPERTIES(BulletExampleBrowserLib PROPERTIES SOVERSION ${BULLET_VERSION})
IF (BUILD_SHARED_LIBS)
		IF (WIN32)
                TARGET_LINK_LIBRARIES(
                        BulletExampleBrowserLib Bullet3Common BulletSoftBody BulletDynamics BulletCollision BulletInverseDynamicsUtils
                        BulletInverseDynamics LinearMath OpenGLWindow gwen
                        ${OPENGL_gl_LIBRARY} ${OPENGL_glu_LIBRARY}
                )
		ELSE(WIN32)
        IF(APPLE)
                 TARGET_LINK_LIBRARIES(
                                BulletExampleBrowserLib Bullet3Common BulletSoftBody BulletDynamics BulletCollision BulletInverseDynamicsUtils
                                BulletInverseDynamics LinearMath OpenGLWindow gwen
                                ${COCOA} ${OPENGL_gl_LIBRARY} ${OPENGL_glu_LIBRARY}
                 )
  			ELSE(APPLE)
                 TARGET_LINK_LIBRARIES(
                                BulletExampleBrowserLib Bullet3Common BulletSoftBody BulletDynamics BulletCollision BulletInverseDynamicsUtils
                                BulletInverseDynamics LinearMath OpenGLWindow gwen
                                pthread dl
                        )
        ENDIF(APPLE)
	ENDIF(WIN32)
ENDIF(BUILD_SHARED_LIBS)

####################
#
# Bullet Example Browser main app
#
####################

INCLUDE_DIRECTORIES(
        .
        ${BULLET_PHYSICS_SOURCE_DIR}/src
        ${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs
)


LINK_LIBRARIES(
        BulletExampleBrowserLib Bullet3Common BulletSoftBody BulletDynamics BulletCollision BulletInverseDynamicsUtils BulletInverseDynamics LinearMath OpenGLWindow gwen
)

IF (WIN32)
	INCLUDE_DIRECTORIES(
		${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs/Glew
	)
	LINK_LIBRARIES(
		${OPENGL_gl_LIBRARY} ${OPENGL_glu_LIBRARY}
	)
	ADD_DEFINITIONS(-DGLEW_STATIC)
ELSE(WIN32)
	IF(APPLE)
                find_library(COCOA NAMES Cocoa)
                MESSAGE(${COCOA})
                link_libraries(${COCOA} ${OPENGL_gl_LIBRARY} ${OPENGL_glu_LIBRARY})
  ELSE(APPLE)
		ADD_DEFINITIONS("-DGLEW_INIT_OPENGL11_FUNCTIONS=1")
		ADD_DEFINITIONS("-DGLEW_STATIC")
		ADD_DEFINITIONS("-DGLEW_DYNAMIC_LOAD_ALL_GLX_FUNCTIONS=1")
		INCLUDE_DIRECTORIES( ${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs/Glew )
		LINK_LIBRARIES( pthread dl)
	ENDIF(APPLE)
ENDIF(WIN32)


SET(ExtendedTutorialsSources
	../ExtendedTutorials/SimpleBox.cpp
	../ExtendedTutorials/MultipleBoxes.cpp
	../ExtendedTutorials/SimpleJoint.cpp
	../ExtendedTutorials/SimpleCloth.cpp
	../ExtendedTutorials/Chain.cpp 
	../ExtendedTutorials/Bridge.cpp 
	../ExtendedTutorials/RigidBodyFromObj.cpp
	../ExtendedTutorials/InclinedPlane.cpp
	../ExtendedTutorials/InclinedPlane.h
	../ExtendedTutorials/NewtonsCradle.cpp
)

SET(BulletExampleBrowser_SRCS
	
	../TinyRenderer/geometry.cpp
	../TinyRenderer/model.cpp
	../TinyRenderer/tgaimage.cpp
	../TinyRenderer/our_gl.cpp
	../TinyRenderer/TinyRenderer.cpp
	../SharedMemory/TinyRendererVisualShapeConverter.cpp
	../SharedMemory/TinyRendererVisualShapeConverter.h
	../RenderingExamples/TinyRendererSetup.cpp
	../SharedMemory/PhysicsServer.cpp
	../SharedMemory/PhysicsClientSharedMemory.cpp
	../SharedMemory/PhysicsClient.cpp
	../SharedMemory/PhysicsClientC_API.cpp
	../SharedMemory/PhysicsServerExample.cpp
	../SharedMemory/PhysicsClientExample.cpp
	../SharedMemory/PosixSharedMemory.cpp
	../SharedMemory/Win32SharedMemory.cpp
	../SharedMemory/InProcessMemory.cpp
	../SharedMemory/PhysicsServerSharedMemory.cpp
	../SharedMemory/PhysicsDirect.cpp
	../SharedMemory/PhysicsDirect.h
	../SharedMemory/PhysicsDirectC_API.cpp
	../SharedMemory/PhysicsDirectC_API.h
	../SharedMemory/SharedMemoryInProcessPhysicsC_API.cpp
	../SharedMemory/PhysicsLoopBack.cpp
	../SharedMemory/PhysicsLoopBack.h
	../SharedMemory/PhysicsLoopBackC_API.cpp
	../SharedMemory/PhysicsLoopBackC_API.h
	../SharedMemory/PhysicsServerCommandProcessor.cpp
	../SharedMemory/PhysicsServerCommandProcessor.h
	../BasicDemo/BasicExample.cpp
	../BasicDemo/BasicExample.h
	../InverseDynamics/InverseDynamicsExample.cpp
	../InverseDynamics/InverseDynamicsExample.h
	../InverseKinematics/InverseKinematicsExample.cpp
	../InverseKinematics/InverseKinematicsExample.h
	../ForkLift/ForkLiftDemo.cpp
	../ForkLift/ForkLiftDemo.h
	../Tutorial/Tutorial.cpp
	../Tutorial/Tutorial.h
	../Tutorial/Dof6ConstraintTutorial.cpp
	../Tutorial/Dof6ConstraintTutorial.h
	../ExtendedTutorials/SimpleBox.cpp
	../ExtendedTutorials/SimpleBox.h
	../ExtendedTutorials/MultipleBoxes.cpp
	../ExtendedTutorials/MultipleBoxes.h
	../ExtendedTutorials/SimpleCloth.cpp
	../ExtendedTutorials/SimpleCloth.h
	../ExtendedTutorials/SimpleJoint.cpp
	../ExtendedTutorials/SimpleJoint.h
	../ExtendedTutorials/NewtonsCradle.cpp
	../ExtendedTutorials/NewtonsCradle.h
	../ExtendedTutorials/NewtonsRopeCradle.cpp
	../ExtendedTutorials/NewtonsRopeCradle.h
	../ExtendedTutorials/InclinedPlane.cpp
	../ExtendedTutorials/InclinedPlane.h
	../ExtendedTutorials/MultiPendulum.cpp
	../ExtendedTutorials/MultiPendulum.h
	../Collision/CollisionSdkC_Api.cpp
	../Collision/CollisionSdkC_Api.h
	../Collision/CollisionTutorialBullet2.cpp
	../Collision/CollisionTutorialBullet2.h
	../Collision/Internal/Bullet2CollisionSdk.cpp
	../Collision/Internal/Bullet2CollisionSdk.h
	../Collision/Internal/CollisionSdkInterface.h
	../Collision/Internal/RealTimeBullet3CollisionSdk.cpp
	../Collision/Internal/RealTimeBullet3CollisionSdk.h
	../GyroscopicDemo/GyroscopicSetup.cpp
	../GyroscopicDemo/GyroscopicSetup.h
	../Planar2D/Planar2D.cpp
	../Planar2D/Planar2D.h
	../RollingFrictionDemo/RollingFrictionDemo.cpp
	../RollingFrictionDemo/RollingFrictionDemo.h
	../FractureDemo/FractureDemo.cpp
	../FractureDemo/btFractureBody.cpp
	../FractureDemo/btFractureDynamicsWorld.cpp
	../FractureDemo/FractureDemo.h
	../FractureDemo/btFractureBody.h
	../FractureDemo/btFractureDynamicsWorld.h
	../DynamicControlDemo/MotorDemo.cpp
	../DynamicControlDemo/MotorDemo.h
	../MultiThreading/MultiThreadingExample.cpp
	../MultiThreading/b3PosixThreadSupport.cpp
	../MultiThreading/b3Win32ThreadSupport.cpp
	../MultiThreading/b3ThreadSupportInterface.cpp
	../RenderingExamples/TimeSeriesCanvas.cpp
	../RenderingExamples/TimeSeriesCanvas.h
	../RenderingExamples/TimeSeriesFontData.cpp
	../RenderingExamples/TimeSeriesFontData.h
	../RoboticsLearning/GripperGraspExample.cpp
	../RoboticsLearning/GripperGraspExample.h
	../RoboticsLearning/b3RobotSimAPI.cpp
	../RoboticsLearning/b3RobotSimAPI.h
	../RoboticsLearning/R2D2GraspExample.cpp
	../RoboticsLearning/R2D2GraspExample.h
	../RoboticsLearning/KukaGraspExample.cpp
	../RoboticsLearning/KukaGraspExample.h
	../RoboticsLearning/IKTrajectoryHelper.cpp
	../RoboticsLearning/IKTrajectoryHelper.h
	../RenderingExamples/CoordinateSystemDemo.cpp
	../RenderingExamples/CoordinateSystemDemo.h
	../RenderingExamples/RaytracerSetup.cpp
	../RenderingExamples/RaytracerSetup.h
	../RenderingExamples/RenderInstancingDemo.cpp
	../RenderingExamples/RenderInstancingDemo.h
	../RenderingExamples/TimeSeriesExample.cpp
	../Benchmarks/BenchmarkDemo.cpp
	../Benchmarks/BenchmarkDemo.h
	../Benchmarks/landscapeData.h
	../Benchmarks/TaruData
	../Raycast/RaytestDemo.cpp
		../Importers/ImportBsp/BspConverter.h
 	../Importers/ImportBullet/SerializeSetup.cpp
	../Importers/ImportBullet/SerializeSetup.h
	../Importers/ImportMeshUtility/b3ImportMeshUtility.cpp
../Importers/ImportMeshUtility/b3ImportMeshUtility.h
	../../Extras/Serialize/BulletWorldImporter/btWorldImporter.cpp
	../../Extras/Serialize/BulletWorldImporter/btBulletWorldImporter.cpp
	../../Extras/Serialize/BulletFileLoader/bChunk.cpp		
	../../Extras/Serialize/BulletFileLoader/bFile.cpp
	../../Extras/Serialize/BulletFileLoader/bDNA.cpp		
	../../Extras/Serialize/BulletFileLoader/btBulletFile.cpp

	../Importers/ImportBsp/BspLoader.h
  ../Importers/ImportBsp/ImportBspExample.h
  ../Importers/ImportColladaDemo/btMatrix4x4.h
  ../Importers/ImportColladaDemo/ColladaGraphicsInstance.h
  ../Importers/ImportColladaDemo/ImportColladaSetup.h
  ../Importers/ImportColladaDemo/LoadMeshFromCollada.h
  ../Importers/ImportObjDemo/ImportObjExample.h
  ../Importers/ImportObjDemo/LoadMeshFromObj.h
  ../Importers/ImportObjDemo/Wavefront2GLInstanceGraphicsShape.h
  ../Importers/ImportSTLDemo/ImportSTLSetup.h
  ../Importers/ImportSTLDemo/LoadMeshFromSTL.h
  ../Importers/ImportURDFDemo/ConvertRigidBodies2MultiBody.h
  ../Importers/ImportURDFDemo/ImportURDFSetup.h
  ../Importers/ImportURDFDemo/URDF2Bullet.h
  ../Importers/ImportURDFDemo/urdf_samples.h
  ../Importers/ImportBsp/BspConverter.cpp
  ../Importers/ImportBsp/BspLoader.cpp
  ../Importers/ImportBsp/ImportBspExample.cpp
  ../Importers/ImportColladaDemo/ImportColladaSetup.cpp
  ../Importers/ImportColladaDemo/LoadMeshFromCollada.cpp
  ../Importers/ImportObjDemo/ImportObjExample.cpp
  ../Importers/ImportObjDemo/LoadMeshFromObj.cpp
  ../Importers/ImportObjDemo/Wavefront2GLInstanceGraphicsShape.cpp
  ../Importers/ImportSTLDemo/ImportSTLSetup.cpp
 ../Importers/ImportSDFDemo/ImportSDFSetup.cpp
  ../Importers/ImportURDFDemo/ImportURDFSetup.cpp
  ../Importers/ImportURDFDemo/URDF2Bullet.cpp
  ../Importers/ImportURDFDemo/MyMultiBodyCreator.cpp
  ../Importers/ImportURDFDemo/MyMultiBodyCreator.h
  ../Importers/ImportURDFDemo/UrdfParser.cpp
  ../Importers/ImportURDFDemo/urdfStringSplit.cpp
 ../Importers/ImportURDFDemo/urdfStringSplit.h
  ../Importers/ImportURDFDemo/BulletUrdfImporter.cpp
  ../Importers/ImportURDFDemo/BulletUrdfImporter.h
  ../VoronoiFracture/VoronoiFractureDemo.cpp
  ../VoronoiFracture/VoronoiFractureDemo.h
  ../VoronoiFracture/btConvexConvexMprAlgorithm.cpp
  ../VoronoiFracture/btConvexConvexMprAlgorithm.h
  ../Vehicles/Hinge2Vehicle.cpp
  ../Vehicles/Hinge2Vehicle.h
  ../MultiBody/Pendulum.cpp
  ../MultiBody/MultiBodySoftContact.cpp
  ../MultiBody/TestJointTorqueSetup.cpp
  ../MultiBody/TestJointTorqueSetup.h
  ../MultiBody/InvertedPendulumPDControl.cpp
  ../MultiBody/InvertedPendulumPDControl.h
 ../MultiBody/MultiBodyConstraintFeedback.cpp
  ../MultiBody/MultiDofDemo.cpp
  ../MultiBody/MultiDofDemo.h
  ../RigidBody/RigidBodySoftContact.cpp
  ../Constraints/TestHingeTorque.cpp
  ../Constraints/TestHingeTorque.h
  ../Constraints/ConstraintDemo.cpp
  ../Constraints/ConstraintDemo.h
  ../Constraints/Dof6Spring2Setup.cpp
  ../Constraints/Dof6Spring2Setup.h
  ../Constraints/ConstraintPhysicsSetup.cpp
  ../Constraints/ConstraintPhysicsSetup.h


	../ThirdPartyLibs/stb_image/stb_image.cpp
	../ThirdPartyLibs/stb_image/stb_image.h
	
	../ThirdPartyLibs/BussIK/Jacobian.cpp
	../ThirdPartyLibs/BussIK/Tree.cpp
	../ThirdPartyLibs/BussIK/Node.cpp
	../ThirdPartyLibs/BussIK/LinearR2.cpp
	../ThirdPartyLibs/BussIK/LinearR3.cpp
	../ThirdPartyLibs/BussIK/LinearR4.cpp
	../ThirdPartyLibs/BussIK/MatrixRmn.cpp
	../ThirdPartyLibs/BussIK/VectorRn.cpp
	../ThirdPartyLibs/BussIK/Misc.cpp
	

	../ThirdPartyLibs/Wavefront/tiny_obj_loader.cpp
	../ThirdPartyLibs/tinyxml/tinystr.cpp
	../ThirdPartyLibs/tinyxml/tinyxml.cpp
	../ThirdPartyLibs/tinyxml/tinyxmlerror.cpp
	../ThirdPartyLibs/tinyxml/tinyxmlparser.cpp

	../ThirdPartyLibs/tinyxml/tinystr.cpp
	../ThirdPartyLibs/tinyxml/tinyxml.cpp
	../ThirdPartyLibs/tinyxml/tinyxmlerror.cpp
	../ThirdPartyLibs/tinyxml/tinyxmlparser.cpp
	InProcessExampleBrowser.cpp
	${BULLET_PHYSICS_SOURCE_DIR}/build3/bullet.rc
)



ADD_EXECUTABLE(App_ExampleBrowser
		main.cpp
		ExampleEntries.cpp
		ExampleEntries.h
		${ExtendedTutorialsSources}
		${BulletExampleBrowser_SRCS} 
)

FILE( MAKE_DIRECTORY "${PROJECT_BINARY_DIR}/data" )

ADD_CUSTOM_COMMAND(
                TARGET App_ExampleBrowser
                POST_BUILD
                COMMAND ${CMAKE_COMMAND} ARGS -E copy_directory ${BULLET_PHYSICS_SOURCE_DIR}/data ${PROJECT_BINARY_DIR}/data
        )


IF (INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
			SET_TARGET_PROPERTIES(App_ExampleBrowser PROPERTIES  DEBUG_POSTFIX "_Debug")
			SET_TARGET_PROPERTIES(App_ExampleBrowser PROPERTIES  MINSIZEREL_POSTFIX "_MinsizeRel")
			SET_TARGET_PROPERTIES(App_ExampleBrowser PROPERTIES  RELWITHDEBINFO_POSTFIX "_RelWithDebugInfo")
ENDIF(INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
