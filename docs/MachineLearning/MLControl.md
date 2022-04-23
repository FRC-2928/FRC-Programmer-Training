# Machine Learning-Based Control
At this point you should have the OAK camera deployed on the Raspberry Pi and sending object detection data to your WPILib program via the Network Tables.  In order to use the data to control the robot you first have to pull it out of the tables.  The `detections` entry of the `ML` Network Table is where data about the detected objects-of-interest are stored.  This entry contains a JSON string that will need to get parsed in your WPILib program.  The structure of the Network Table and JSON string looks like this:

![Generating Dataset](../images/FRCMachineLearning/FRCMachineLearning.012.jpeg)

WPILib uses the [Jackson](https://github.com/FasterXML/jackson) JSON parser so these libraries must be imported:

    import com.fasterxml.jackson.core.JsonProcessingException;
    import com.fasterxml.jackson.databind.ObjectMapper;

You have to create a Java class that exactly matches the JSON string that you want to parse (see the graphic above).  Once you have that class then parsing the JSON string is very simple.  Here's an example method:

    public void parseDetections(String json) {
        //create ObjectMapper instance
        ObjectMapper mapper = new ObjectMapper();     
        try {
            //JSON string to Java Object
            m_detections = mapper.readValue(json, Detections[].class);
        } catch (JsonProcessingException e) {
            System.out.println(e);
            System.out.println(json);
        } 	 
    }

And the `m_detections` variable looks like this:

    Detections[] m_detections;

Now you can separate everything out like this:

    // Get bounding box components
    public Map<String, Double> getBox() {
        return m_detections[0].box; 
    }

    public Double getXMin() {
        return getBox().get("xmin");
    }

    ...

A full code example showing how to parse the JSON string can be found [here](https://github.com/FRC-2928/RomiExamples/blob/main/BasicML/src/main/java/frc/robot/subsystems/Vision.java)    

## References
- [Jackson JSON](https://github.com/FasterXML/jackson) parser

- [Jackson JSON](https://www.journaldev.com/2324/jackson-json-java-parser-api-example-tutorial) Java Tutorial