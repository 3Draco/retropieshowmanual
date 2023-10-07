# retropieshowmanual

1 - Install webfs and Configure Web Server:
Install the webfs web server by running the following command:

sudo apt-get -y install webfs

--------------------------------------------------------

2 - Change the Web Server settings:
Edit the webfsd.conf file. You can use a text editor of your choice, e.g., nano:

sudo nano /etc/webfs/webfsd.conf

port="80"
web_root="/home/pi/RetroPie/manual/www/"
web_index="index.html"

--------------------------------------------------------

3 - Create Folders and Copy Files:
Create the necessary folders if they don't already exist:

mkdir -p /home/pi/RetroPie/manual/www/
mkdir -p /home/pi/RetroPie/manual/manual/

Copy index.html to the www folder.
Copy nomanual.pdf and retropie.pdf to the manual folder.

--------------------------------------------------------

4 - Insert the following in to runcommand-onstart.sh

	# Enable debug mode (set DEBUG to true to see debug outputs)
	DEBUG=false

	# Path to the directory where the manuals (PDFs) are located
	manuals_directory="/home/pi/RetroPie/roms/$1/media/manuals"

	# Path to the target directory where the manual should be copied
	target_directory="/home/pi/RetroPie/manual/www"

	# Extract the game name from the full path to the ROM file
	game_name=$(basename "$3" | sed 's/\.[^.]*$//')

	# Set the path to the source file (manual)
	source_file="$manuals_directory/$game_name.pdf"

	# Check if the source file (manual) exists
	if [ -e "$source_file" ]; then
		# Set the target directory and filename for the copied manual
		target_file="$target_directory/manual.pdf"

		# Copy the source file (manual) to the target directory and save it as "manual.pdf" (overwrite if already exists)
		cp "$source_file" "$target_file"

		if [ "$DEBUG" = true ]; then
			# Display debug output
			echo "DEBUG: Manual found"
			echo "DEBUG: The manual $source_file was successfully copied and saved as manual.pdf to $target_file."
			echo "DEBUG: Game name: $3"
			sleep 30s
		fi
  
		else
		# If the source file (manual) was not found, copy and rename a PDF with the name "nomanual.pdf" from another directory
		nomanual_source="/home/pi/RetroPie/manual/nomanual.pdf"
		nomanual_target="$target_directory/manual.pdf"

		# Copy the replacement PDF (nomanual.pdf) to the target directory and save it as "manual.pdf" (overwrite if already exists)
		cp "$nomanual_source" "$nomanual_target"

		if [ "$DEBUG" = true ]; then
			# Display debug output
			echo "DEBUG: No manual found"
			echo "DEBUG: A default manual $nomanual_source was saved as manual.pdf to $nomanual_target."
			echo "DEBUG: Game name: $3"
			sleep 30s
		fi

--------------------------------------------------------
  
5 - Insert the following line in to runcommand-onend.sh

cp /home/pi/RetroPie/manual/retropie.pdf /home/pi/RetroPie/manual/www/manual.pdf > /dev/null 2>&1

--------------------------------------------------------

7 - Make the "runcommand-onend.sh" and "runcommand-onstart.sh" file executable using the chmod command:

chmod +x runcommand-onend.sh

chmod +x runcommand-onstart.sh




















