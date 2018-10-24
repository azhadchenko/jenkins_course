#!/usr/bin/env groovy

def linux_compile_script(String node_name) { 	
	bash """
		gcc -o ${node_name}.out helloworld.c
	"""
	stash name: "build_${node_name}", includes "*.out"
}

timestamps {
	stage('build') {
		parallel(
			'build_win': {
				node('windows') {
					checkout scm
					String vsvars_bat = 'Microsoft Visual Studio 12.0\\VC\\vcvarsall.bat'
					bat """
						call %ProgramFiles(X86)%\\${vsvars_bat}"
						x86
						cl.exe helloworld.c
					"""

					stash name: "build_win", includes: "*.exe"
				}
			}

			'build_linux' : {

				node('linux') {
					linux_compile_script('linux')
				}
			}

			'build_mac' : {

				node('mac-mini') {
					linux_compile_script('mac')
				}
			}
		)
	}

	stage('store') {
	
	}
}
