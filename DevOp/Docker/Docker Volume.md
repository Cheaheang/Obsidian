To connect your volume to local storage
Type 1
	docker run -v /home/mount/data:var/lib/mysql/data
		/home/mount/data : your local directory
		var/lib/mysql/data : your container store data
Type 2 (Anonymous volume)
	docker run -v var/lib/mysql/data
		This Cmd, docker will create volume by itself
Type 3(Name volume)
	docker run -v name:var/lib/mysql/data
 
![[Pasted image 20251223120615.png]]