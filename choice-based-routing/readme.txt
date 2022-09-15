Please do below changes to make application runnable.

1. ExternalOutboundChannel.java

package com.lti;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/v1")
public class ExternalOutboundChannel {

	@PostMapping("/hot")
	String hot(@RequestBody String str) {
		System.out.println("External channel 1 : message recevied !  " + str);
		return str;
	}
	
	@PostMapping("/cold")
	String cold(@RequestBody String str) {
		System.out.println("External channel other : message recevied !  " + str);
		return str;
	}
}

2. Add below http outbound channel

	<int-http:outbound-channel-adapter
		id="hot" url="http://localhost:8080/v1/hot"
		channel="1" http-method="POST" charset="UTF-8"
		auto-startup="true" />
		
		<int-http:outbound-channel-adapter
		id="cold" url="http://localhost:8080/v1/cold"
		channel="other" http-method="POST" charset="UTF-8"
		auto-startup="true" />
