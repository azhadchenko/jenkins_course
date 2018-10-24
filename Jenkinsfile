#!/usr/bin/env groovy

timestamps {
	stage('build') {
		parallel(
			'build_win': {
				node('windowns') {
					checkout scm
				}
			}
		)
	}

	stage('store') {
	
	}
}
