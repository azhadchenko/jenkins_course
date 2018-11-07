#!/usr/bin/env groovy

Boolean _build_mac = true
try {
	_build_mac = build_mac.toBoolean()
} catch (MissingPropertyException ignore) {
	_build_mac = true
}

Boolean _build_linux = true
try {
	_build_linux = build_linux.toBoolean()
} catch (MissingPropertyException ignore) {
	_build_linux = true
}

Boolean _build_win = true
try {
	_build_win = build_win.toBoolean()
} catch (MissingPropertyException ignore) {
	_build_win = true
}


def unix_compile_script(node_name) {
	node(node_name) {
		checkout scm 	
		sh """
			#!/bin/bash
			gcc -o ${node_name}.out helloworld.c
		"""
		stash name: "build_${node_name}", includes: "*.out"
		deleteDir()
	}
}

def win_compile_script() {
    node('windows') {
		checkout scm
		String vsvars_bat = 'Microsoft Visual Studio 12.0\\VC\\vcvarsall.bat'
		bat """
		    call "%ProgramFiles(X86)%\\${vsvars_bat}" x86
		    cl.exe helloworld.c
		"""

		stash name: "build_win", includes: "*.exe"
		deleteDir()
	}
}

timestamps {
	stage('build') {
		parallel(
			'build_win': {
			    if(_build_win == true) {
			        win_compile_script()
			    }
			},

			'build_linux' : {
			    if(_build_linux == true) {
				    unix_compile_script('linux')
			    }
			},

			'build_mac' : {
			    if(_build_mac == true) {
				    unix_compile_script('mac')
			    }
			}
		)
	}

	stage('store') {
	    node("master") {
	        if(_build_mac == true) { unstash "build_mac" }
	        if(_build_win == true) { unstash "build_win" }
	        if(_build_linux == true) { unstash "build_linux" }
	        archiveArtifacts artifacts: '*'
	        deleteDir()
	    }
	
	}
}