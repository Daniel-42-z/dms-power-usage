#!/bin/bash

bat_info_path="/sys/class/power_supply/BAT0"

# Check if the direct power reading is available
if [ -f "${bat_info_path}/power_now" ]; then
	# Direct power reading exists, so read and convert it
	# The value is in microwatts, so divide by 1,000,000 to get watts
	power_microwatts=$(cat "${bat_info_path}/power_now" | xargs)
	power_watts=$(echo "${power_microwatts}" | awk '{printf "%.2f", $1/1e6}')
	echo "${power_watts}W"
	exit 0
fi

# Direct power reading not available, check for voltage file
if [ -f "${bat_info_path}/voltage_now" ]; then
	# Voltage file exists, now check for current file
	if [ -f "${bat_info_path}/current_now" ]; then
		# Both voltage and current files exist
		# Read voltage in microvolts and convert to volts
		voltage_microvolts=$(cat "${bat_info_path}/voltage_now" | xargs)
		voltage_volts=$(echo "${voltage_microvolts}" | awk '{print $1/1e6}')

		# Read current in microamps and convert to amps
		current_microamps=$(cat "${bat_info_path}/current_now" | xargs)
		current_amps=$(echo "${current_microamps}" | awk '{print $1/1e6}')

		# Calculate power: Power (watts) = Voltage (volts) × Current (amps)
		power_watts=$(echo "${voltage_volts} ${current_amps}" | awk '{printf "%.2f", $1 * $2}')
		echo "${power_watts}W"
		exit 0
	fi
fi

# Neither direct power nor voltage/current combination is available
echo "N/A"
exit 1
