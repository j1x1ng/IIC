All codes are under GPL_v3 license.

However, due to the champion's rule, I can not open my sources until it has finished.

So any upload before the date should be encrypted. ( by zip )

Forgive me.

----------------------------------------------------------------------------------------------

Usage :
	to classify a real item.
	in my case, the item is classified into four types : plastic, metal, glass and paper.
	what's more, my items are yin liao bao zhuang.
	you may modified it into other situation.

	Under GPL_V3. Hope my software to be useful.

	Thanks.

----------------------------------------------------------------------------------------------

Code List : cboxctrl.h cboxctrl.cpp audio_cut.h ac.cpp video_cut.h vc.cpp bys.h bys.cpp

Classes :

	cbox --> to rotate platform
		to input / output items

	ac  -->	to record a short audio cut
		return ():to "tongji" the 4-yuan-zu(pin pu fen bu) 4 x 2
			represented as a 8-bit interger(unsigned)
			[256]
		// bei zhu : ac use arecord as a backgroud process, which mmap a block of memory to share it with main::process()

	vc  -->	to capture a snapshot
		return ():determine the touming area / total area
			[4]
		return ():its sharpness ( edge )
			[4]
		return ():time to get stable | wen ding shi jian
			[4]
		return ():lightness ( which showes its absorb type under hongwai (infare?) )
			[4]

	BeiYeSi -->	data : 16-bit status (32K) * 2-bit type(4) * Len8Byte(64bit) Double = 1MB

			read from data file
			trainning
			to judge

Process :

	1)	main::process() ask #vc#::cut()
	2)	once #vc# inputed a cut, ask #ac#::cut()
	3)	#ac#::value() returns a 8-bit status
		#vc#::value() returns a 8-bit status ( contains 2-bit areaK, 2-bit sharpness, 2-bit stabletime, 2-bit lightness)
	4)	main::process() gave the 16-bit status struct to #BeiYeSi#
	5)	#BeiYeSi# returns a 3-bit judgement ( which represented the type of item or its a unknown ...)
	6a)	if unknown do not go back to 1) until item is removed
	6b)	if known, main::process() ask #cbox#::rotate() to output the item.
	7)	go back to 1)
	
Requirement :
	Raspberry Pi Model B ( Made in China, Red Board )
	Raspberry Pi Night Mode Camera

	26-pin pai xian. 
	&& san ji guan + ji dian qi kong zhi ban & zhi liu dian ji & hardware BOX
	&& 940nm LED.

	wiringPi library
	raspiCam library
