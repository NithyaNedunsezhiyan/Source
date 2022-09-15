url: http://localhost:8080/json
input data: ["Tabrej", "Aigis","Nirdesh"]

Please do below changes to make application runnable.

1. create ServiceHandler.java and ExternalOutboundChannel.java


package com.lti;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import java.util.List;

@RestController
@RequestMapping("/v1")
public class ExternalOutboundChannel {

	@PostMapping("/outbound")
	List<String> outbound(@RequestBody List<String> list) {
		System.out.print("External channel : message recevied !  " + list);
		return list;
	}

}

package com.lti;
import java.util.List;
public class ServiceHandler {
	public List<String>  handle(List<String> list) {
		System.out.println("ServiceHandler : list Object data : " + list);
		return list;
	}
}

2. Add below http outbound channel

	<integration:channel
		id="serviceActivatorOutputChannel" />
	<int:service-activator id="inboundEnpoint"
		method="handle" output-channel="serviceActivatorOutputChannel"
		ref="handler" input-channel="outputChannel" auto-startup="true" />
	<bean id="handler" class="com.lti.ServiceHandler" />
	<int-http:outbound-channel-adapter
		id="out" url="http://localhost:8080/v1/outbound"
		channel="serviceActivatorOutputChannel" http-method="POST"
		charset="UTF-8" auto-startup="true" />