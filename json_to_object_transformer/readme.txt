url: http://localhost:8080/json
input data: {"id":"23423423","name":"Test Name"}

Please do below changes to make application runnable.

1. create User.java,ServiceHandler.java and ExternalOutboundChannel.java

package com.lti;
public class User {
	private String id;
	private String name;
	public String getId() {
		return id;
	}

	public void setId(String id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	@Override
	public String toString() {
		return "User [id=" + id + ", name=" + name + "]";
	}
}

package com.lti;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/v1")
public class ExternalOutboundChannel {

	@PostMapping("/outbound")
	String outbound(@RequestBody User user) {
		System.out.print("External channel : message recevied !  " + user);
		return user.getName();
	}

}

package com.lti;
public class ServiceHandler {
	public User handle(User user) {
		System.out.println("ServiceHandler : User Object data : " + user);
		return user;
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