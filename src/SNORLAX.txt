:DCS6
"0000F3CFFFFFFFFFFFFF7FFEFFFFF18FE007EC37D24B400363C6F81EFFFFF80F"    //icon
:
GridOff:AxesOff:0->Xmin:94->Xmax    //prepare graph screen
~62->Ymin:0->Ymax
FnOff
"FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFC00007FF000000FC0000003C00000010000000000000000000000000000000000000000000000008000000180000001C0000007F000001FFF0000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFE0F1FFFFE061FFFFE001FFFF80001FFF00000FFF00000FFF00000FFF80003FFF00003FFE00001FFE00001FFE00001FFFE0001FFFE0001FFE00000FF800000FF000000FF0000007E0000007E0000007E0000007E0000007E0000007E000000FF000000FF000001FF800003FFE0300FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFC3FFFFFF003FFFFF000FFFFF000FFFFF000FFFFC003FFFF0000FFFE00007FFE00007FFC00003FFC00003FF800001000000010000000100000001000000010000000100000001000000018000000380000003C0000007C0000007E000000FF000003FF80000FFFFFFFFFFFFFFFFFFFC3FFFFFF81FFE0FF80FF80FF807E00FF807800FF803000FF803000FF803001FF800001FF800003FF800003FFC00007FFC0000FFFC0000FFFC0000FFFC0000FFFC0000FFFC00007FFC00003FFC00003FF800003FF800003FF800001FF800000FFE000007FF800007FF000007FE00000FFE00011FFE0001FFFE0001FFFFFFFFFF"->Str1    //masks
"0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001F99F0006D669E019A59A1821CD34A4400E394641000006620820066510514E3AAAAABC3D55557C1FEBABF007FFFFC0007EFE00000000000000000000000000000000000000000000000000000E040000050C0000029400003E55C00063666000192980000F320000332F000041508000FDFF400007EF8000075380000A81C000080AC000FD54E0034EA16004420AE006E7547009F9A070099185700F91AAF008E9D060052702E0062757C003FFFF8000F87E0000000000000000000000000000000000000000000000000000000000000000000000180000007F80000006E000000C0000007E000001D580000702E0000C557000094850001A22B8001155580035045C7FE22AAC421FFD5C6004595C41087AAC6020BD5C2000F57830017AB81806F5F01C0BD7F00F7FEFE007FFFF8003FE7E00000000000000000000000000000000000180000003C000E003E003E003F00FE003B03DE0031870C0031861C003084180030FC3800110870001C00C0000E0640001B0B20001F0F2000164620001000200018E1C0000863D0001603F8001FFF880021FE100031AC78000E89BC0003017E000101DF0003038E000478840008C440000F03C000000000000000000"->Str2    /sprites
2->dim(|LYUM:Fill(0,|LYUM   //prepare list for score
ClrDraw:ClrHome							//clears screen for menu
RecallPic 1									//menu screen
identity(5,"A8A8F87020202000",27,32,1,8,2,0,1			//fork
32->Y:0->K																				//initialize variables
While K!=21																				//runs until you press 2nd
	Repeat K																				//getkey loop
		getKey->K
	End
	If K=25 and Y=41:Then																//if you press the up button and can move the cursor (fork) up
		identity(5,"A8A8F87020202000",27,Y,1,8,3,0,1			//erases old fork
		32->Y																							//changes Y
		identity(5,"A8A8F87020202000",27,Y,1,8,2,0,1			//draws new fork
	End
	If K=34 and Y=32:Then																//if you press the down button and can move the fork down
		identity(5,"A8A8F87020202000",27,Y,1,8,3,0,1			//erases old fork
		41->Y																							//changes Y
		identity(5,"A8A8F87020202000",27,Y,1,8,2,0,1			//draws new fork
	End
End
If Y=41:Goto Q																				//if you selected "Quit" then the program quits, otherwise it assumes you pressed start.
ClrDraw   //clear the graph screen
Lbl 1
randInt(0,3)->A   //random sprite
Text(~1,1,74,|LYUM(1    //display yums
Text(1,2,|LYUM(2    //display score
For(Y,~20,10,10   //falling loop
	RecallPic 2    //redraw background
	sub(Str2,A*256+1,256   //select sprite to display
	identity(5,Ans,32,Y,4,32,3,0,1   //display sprite
	identity(5,Ans,32,Y,4,32,3,0,1   //clear sprite
End
sub(Str1,A*256+1,256    //select mask
identity(5,Ans,32,Y-6,4,32,1,0,1    //display mask
sub(Str2,A*256+1,256    //select sprite
identity(5,Ans,32,Y-6,4,32,3,0,1    //display sprite
For(X,87,~1,~8    //arrow loop
	identity(5,"1C224282828242221C",X,55,1,9,3,0,1   //draw arrow
	For(T,1,15-(iPart(|LYUM(1)/30)+1   //pause loop which speeds up
	End
	identity(5,"1C224282828242221C",X,55,1,9,3,0,1   //clear arrow
	getKey->K    //check for key press
	If K=21:Goto G   //if you press 2nd end the loop
End
Lbl G   //when the loop is over
If X<=0 xor A=3:Then    //if you ate the pichu or missed the food
	identity(8,"FFFFFFFFFFFFFFFF",0,0,3,1    //inverts graph screen
	For(T,1,350					//wait a bit for the effect
	End									//stop waiting because people are impatient
	ClrDraw							//clear the screen
	RecallPic 3					//game over
	Pause 							//actually paused
	Goto S							//when you press enter the game resumes again
End   								//after this it assumes you didn't do anything bad like eat a pichu or miss a food

//scoring

|LYUM(1)+(A!=3)->|LYUM(1   									//add a yum, but only for food
If X>=55:|LYUM(2)+10->|LYUM(2)    					//if you got a great then add 10 to the score
If X<=54 and X>=26:|LYUM(2)+5->|LYUM(2)   	//if you got a good then add 5 to the score
If X<=25:|LYUM(2)+1->|LYUM(2)   						//if you got a ok then add 1 to the score
sub(Str1,A*256+1,256    										//select sprite to remove
identity(5,Ans,32,Y-6,4,32,1,0,1 				  	//remove sprite
RecallPic 2  																//redraw background
Goto 1 																	  	//restart game loop
Lbl Q																				//game is stopped
ClrDraw:ClrHome															//clears screen
DelVar TDelVar YDelVar XDelVar A						//deleting variables
Stop																				//STOP
