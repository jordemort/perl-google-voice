Google Voice library for Perl.  

 - Handles all google voice functions
 - No parsing required - all data available in perl objects
 - Only two pre-requisites:
 	- Mojolicious
	- IO::Socket::SSL

Example

	use Google::Voice;
	
	my $g = Google::Voice->new->login( 'user', 'pass' );
	
	# sms conversation
	foreach my $sms ( $g->sms ) {
		print $sms->name;
		print $_->time , ':', $_->text, "\n" foreach $sms->messages;
		
		$sms->delete;
	}
	
	$g->send_sms( '+15555555555' => 'Hello friend!' );
	
	# connect call & cancel it
	my $call = $g->call( '+15555555555' => '+14444444444' );
	$call->cancel;
	
	# loop through voicemail messages
	foreach my $vm ( $g->voicemail ) {

		# Name, number, and transcribed text
		print $vm->name . "\n";
		print $vm->meta->{phoneNumber} . "\n";
		print $vm->text . "\n";
		
		# Download mp3
		$vm->download->move_to( $vm->id . '.mp3' );

		# Delete
		$vm->delete;
	}
