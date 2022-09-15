Please do below changes to make application runnable.

1. create User.java,ObjectFactory.java and ExternalOutboundChannel.java

package com.lti.model;

import java.io.Serializable;
import javax.xml.bind.annotation.XmlAccessType;
import javax.xml.bind.annotation.XmlAccessorType;
import javax.xml.bind.annotation.XmlElement;
import javax.xml.bind.annotation.XmlRootElement;
import javax.xml.bind.annotation.XmlType;

@XmlRootElement(name="user")
@XmlAccessorType(XmlAccessType.FIELD)
@XmlType(name = "", propOrder = { "id", "name" })
public class User implements Serializable{
	private static final long serialVersionUID = 1L;

	@XmlElement(name="id" ,required = true)
	private String id;
	
	@XmlElement(name="name",required = true)
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


package com.lti.model;
import javax.xml.bind.annotation.XmlRegistry;
@XmlRegistry
public class ObjectFactory {
    /**
     * Create a new ObjectFactory that can be used to create new instances of schema derived classes for package: com.intertech.domain
     * 
     */
    public ObjectFactory() {
    }

    /**
     * Create an instance of {@link Shiporder }
     * 
     */
    public User createUser() {
        return new User();
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
	String outbound(@RequestBody String str) {
		System.out.print("External channel : message recevied !  " + str);
		return str;
	}
}

2. Add below http outbound channel

<integration:channel id="out" />
	<!-- Step-6 Print json object in external service -->
	<int-http:outbound-channel-adapter
		id="externalService" url="http://localhost:8080/v1/outbound"
		channel="out" http-method="POST" charset="UTF-8"
		auto-startup="true" />