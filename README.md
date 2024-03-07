Practical 1 - CloudSimExample1 public static void main(String[] args) {
Log.printLine("Starting CloudSimExample1...");
try {
// First step: Initialize the CloudSim package. int num_user = 1;
Calendar calendar = Calendar.getInstance(); boolean trace_flag = false;
CloudSim.init(num_user, calendar, trace_flag);
// Second step: Create Datacenters
Datacenter datacenter0 = createDatacenter("Datacenter_0");
// Third step: Create Broker
DatacenterBroker broker = createBroker(); int brokerId = broker.getId();
// Fourth step: Create one virtual machine vmlist = new ArrayList<Vm>();
// VM description int vmid = 0; int mips = 1000; long size = 10000; // image size (MB) int ram = 512; // vm memory (MB) long bw = 1000;
int pesNumber = 1; // number of cpus String vmm = "Xen"; // VMM name
// create VM
Vm vm = new Vm(vmid, brokerId, mips, pesNumber, ram,
bw, size, vmm, new CloudletSchedulerTimeShared());
vmlist.add(vm); broker.submitVmList(vmlist);
// Fifth step: Create one Cloudlet cloudletList = new ArrayList<Cloudlet>();
// Cloudlet properties int id = 0; long length = 400000; long fileSize = 300; long outputSize = 300;
UtilizationModel utilizationModel = new
UtilizationModelFull();
Cloudlet cloudlet = new Cloudlet(id, length, pesNumber,
fileSize, outputSize, utilizationModel, utilizationModel, utilizationModel);
cloudlet.setUserId(brokerId); cloudlet.setVmId(vmid); cloudletList.add(cloudlet); broker.submitCloudletList(cloudletList);
// Sixth step: Starts the simulation
CloudSim.startSimulation();
CloudSim.stopSimulation();
//Final step: Print results when simulation is over
List<Cloudlet> newList = broker.getCloudletReceivedList(); printCloudletList(newList);
Log.printLine("CloudSimExample1 finished!");
} catch (Exception e) {
e.printStackTrace();
Log.printLine("Unwanted errors happen"); }
}


Practical 2 - An example showing how to create scalable simulation CloudSimExample6 public static void main(String[] args) {
Log.printLine("Starting CloudSimExample6...");
try {
// First step: Initialize the CloudSim package. int num_user = 1; // number of grid users Calendar calendar = Calendar.getInstance(); boolean trace_flag = false;
CloudSim.init(num_user, calendar, trace_flag);
// Second step: Create Datacenters
@SuppressWarnings("unused")
Datacenter datacenter0 = createDatacenter("Datacenter_0"); @SuppressWarnings("unused")
Datacenter datacenter1 = createDatacenter("Datacenter_1");
//Third step: Create Broker
DatacenterBroker broker = createBroker(); int brokerId = broker.getId();
//Fourth step: Create VMs and Cloudlets and send them to
broker vmlist = createVM(brokerId,20); //creating 20 vms cloudletList = createCloudlet(brokerId,40); // creating 40
cloudlets
broker.submitVmList(vmlist); broker.submitCloudletList(cloudletList);
// Fifth step: Starts the simulation
CloudSim.startSimulation();
// Final step: Print results when simulation is over
List<Cloudlet> newList = broker.getCloudletReceivedList(); CloudSim.stopSimulation(); printCloudletList(newList);
Log.printLine("CloudSimExample6 finished!");
} catch (Exception e)
{
e.printStackTrace();
Log.printLine("The simulation has been terminated due to an
unexpected error");
}
} 


Practical 3 - NetworkExample2 public static void main(String[] args) {
Log.printLine("Starting NetworkExample2...");
try {
// First step: Initialize the CloudSim package. int num_user = 1; // number of cloud users Calendar calendar = Calendar.getInstance(); boolean trace_flag = false;
CloudSim.init(num_user, calendar, trace_flag);
// Second step: Create Datacenters
//Datacenters are the resource providers in CloudSim.
Datacenter datacenter0 = createDatacenter("Datacenter_0");
Datacenter datacenter1 = createDatacenter("Datacenter_1");
//Third step: Create Broker
DatacenterBroker broker = createBroker(); int brokerId = broker.getId();
//Fourth step: Create one virtual machine vmlist = new ArrayList<Vm>();
//VM description int vmid = 0; int mips = 250; long size = 10000; //image size (MB) int ram = 512; //vm memory (MB) long bw = 1000;
int pesNumber = 1; //number of cpus String vmm = "Xen"; //VMM name
//create two VMs
Vm vm1 = new Vm(vmid, brokerId, mips, pesNumber, ram,
bw, size, vmm, new CloudletSchedulerTimeShared());
vmid++;
Vm vm2 = new Vm(vmid, brokerId, mips, pesNumber, ram,
bw, size, vmm, new CloudletSchedulerTimeShared());
//add the VMs to the vmList vmlist.add(vm1); vmlist.add(vm2);
//submit vm list to the broker broker.submitVmList(vmlist);
//Fifth step: Create two Cloudlets cloudletList = new ArrayList<Cloudlet>();
//Cloudlet properties int id = 0; long length = 40000; long fileSize = 300; long outputSize = 300;
UtilizationModel utilizationModel = new
UtilizationModelFull();
Cloudlet cloudlet1 = new Cloudlet(id, length, pesNumber,
fileSize, outputSize, utilizationModel, utilizationModel, utilizationModel);
cloudlet1.setUserId(brokerId);
id++;
Cloudlet cloudlet2 = new Cloudlet(id, length, pesNumber,
fileSize, outputSize, utilizationModel, utilizationModel, utilizationModel);
cloudlet2.setUserId(brokerId);
//add the cloudlets to the list cloudletList.add(cloudlet1); cloudletList.add(cloudlet2); broker.submitCloudletList(cloudletList); broker.bindCloudletToVm(cloudlet1.getCloudletId(),vm1.getId()); broker.bindCloudletToVm(cloudlet2.getCloudletId(),vm2.getId());
//Sixth step: configure network
//load the network topology file
NetworkTopology.buildNetworkTopology("topology.brite");
//Datacenter0 will correspond to BRITE node 0 int briteNode=0;
NetworkTopology.mapNode(datacenter0.getId(),briteNode);
//Datacenter1 will correspond to BRITE node 2 briteNode=2;
NetworkTopology.mapNode(datacenter1.getId(),briteNode);
//Broker will correspond to BRITE node 3 briteNode=3;
NetworkTopology.mapNode(broker.getId(),briteNode);
// Sixth step: Starts the simulation
CloudSim.startSimulation();
// Final step: Print results when simulation is over
List<Cloudlet> newList = broker.getCloudletReceivedList(); CloudSim.stopSimulation(); printCloudletList(newList);
Log.printLine("NetworkExample2 finished!");
} catch (Exception e) {
e.printStackTrace();
Log.printLine("The simulation has been terminated due to an
unexpected error");
}
} 


Practical 4 - NetworkExample3 public static void main(String[] args) {
Log.printLine("Starting NetworkExample3...");
try {
// First step: Initialize the CloudSim package. int num_user = 2; // number of cloud users Calendar calendar = Calendar.getInstance(); boolean trace_flag = false; // mean trace events
// Initialize the CloudSim library
CloudSim.init(num_user, calendar, trace_flag);
// Second step: Create Datacenters
Datacenter datacenter0 = createDatacenter("Datacenter_0");
Datacenter datacenter1 = createDatacenter("Datacenter_1");
//Third step: Create Brokers
DatacenterBroker broker1 = createBroker(1); int brokerId1 = broker1.getId();
DatacenterBroker broker2 = createBroker(2); int brokerId2 = broker2.getId();
//Fourth step: Create one virtual machine for each
broker/user vmlist1 = new ArrayList<Vm>(); vmlist2 = new ArrayList<Vm>();
//VM description int vmid = 0; long size = 10000; //image size (MB) int mips = 250; int ram = 512; //vm memory (MB) long bw = 1000; int pesNumber = 1; //number of cpus String vmm = "Xen"; //VMM name
//create two VMs: the first one belongs to user1
Vm vm1 = new Vm(vmid, brokerId1, mips, pesNumber,
ram, bw, size, vmm, new CloudletSchedulerTimeShared());
//the second VM: this one belongs to user2
Vm vm2 = new Vm(vmid, brokerId2, mips, pesNumber,
ram, bw, size, vmm, new CloudletSchedulerTimeShared());
//add the VMs to the vmlists vmlist1.add(vm1); vmlist2.add(vm2);
//submit vm list to the broker broker1.submitVmList(vmlist1); broker2.submitVmList(vmlist2);
//Fifth step: Create two Cloudlets cloudletList1 = new ArrayList<Cloudlet>(); cloudletList2 = new ArrayList<Cloudlet>();
//Cloudlet properties int id = 0; long length = 40000; long fileSize = 300; long outputSize = 300;
UtilizationModel utilizationModel = new
UtilizationModelFull();
Cloudlet cloudlet1 = new Cloudlet(id, length, pesNumber,
fileSize, outputSize, utilizationModel, utilizationModel, utilizationModel);
cloudlet1.setUserId(brokerId1);
Cloudlet cloudlet2 = new Cloudlet(id, length, pesNumber,
fileSize, outputSize, utilizationModel, utilizationModel, utilizationModel);
cloudlet2.setUserId(brokerId2);
cloudletList1.add(cloudlet1); cloudletList2.add(cloudlet2);
//submit cloudlet list to the brokers broker1.submitCloudletList(cloudletList1); broker2.submitCloudletList(cloudletList2);
//Sixth step: configure network
//load the network topology file
NetworkTopology.buildNetworkTopology("topology.brite");
//Datacenter0 will correspond to BRITE node 0 int briteNode=0;
NetworkTopology.mapNode(datacenter0.getId(),briteNode);
//Datacenter1 will correspond to BRITE node 2 briteNode=2;
NetworkTopology.mapNode(datacenter1.getId(),briteNode);
//Broker1 will correspond to BRITE node 3 briteNode=3;
NetworkTopology.mapNode(broker1.getId(),briteNode);
//Broker2 will correspond to BRITE node 4 briteNode=4;
NetworkTopology.mapNode(broker2.getId(),briteNode);
// Sixth step: Starts the simulation
CloudSim.startSimulation();
// Final step: Print results when simulation is over
List<Cloudlet> newList1 =
broker1.getCloudletReceivedList();
List<Cloudlet> newList2 =
broker2.getCloudletReceivedList();
CloudSim.stopSimulation();
Log.print("=============> User "+brokerId1+"	"); printCloudletList(newList1);
Log.print("=============> User "+brokerId2+"	"); printCloudletList(newList2);
Log.printLine("NetworkExample3 finished!");
} catch (Exception e) {
e.printStackTrace();
Log.printLine("The simulation has been terminated due to an
unexpected error");
}
}
