% Clear workspace
clc;
clear;

% Create a new Simulink model
modelName = 'EnginePerformanceModel';
open_system(new_system(modelName));

% Define constants
bore_diameter = 4.3; % Bore diameter (cm)
stroke_length = 5.35; % Stroke length (cm)
thickness_gasket = 0.2; % Thickness of engine head gasket (cm)
upper_side_length = 0.2; % Length left on upper side of piston (cm)
void_volume = 2.638; % Intersected void volume (cc)
fuel_consumption_volume = 3; % Fuel consumed in ml
time_duration = 3 + (14 / 1000); % Time duration in minutes
fuel_density = 0.703; % Density of Octane 93 (g/ml)

% Create Constant blocks for inputs
add_block('simulink/Commonly Used Blocks/Constant', [modelName, '/Bore Diameter'], 'Value', num2str(bore_diameter));
add_block('simulink/Commonly Used Blocks/Constant', [modelName, '/Stroke Length'], 'Value', num2str(stroke_length));
add_block('simulink/Commonly Used Blocks/Constant', [modelName, '/Gasket Thickness'], 'Value', num2str(thickness_gasket));
add_block('simulink/Commonly Used Blocks/Constant', [modelName, '/Upper Side Length'], 'Value', num2str(upper_side_length));
add_block('simulink/Commonly Used Blocks/Constant', [modelName, '/Void Volume'], 'Value', num2str(void_volume));
add_block('simulink/Commonly Used Blocks/Constant', [modelName, '/Fuel Consumption Volume'], 'Value', num2str(fuel_consumption_volume));
add_block('simulink/Commonly Used Blocks/Constant', [modelName, '/Time Duration'], 'Value', num2str(time_duration));
add_block('simulink/Commonly Used Blocks/Constant', [modelName, '/Fuel Density'], 'Value', num2str(fuel_density));

% Create a Math Function block for Displacement Volume
add_block('simulink/Math Operations/Product', [modelName, '/Displacement Volume']);
set_param([modelName, '/Displacement Volume'], 'Inputs', '2');
% Connect the blocks to calculate displacement volume
add_line(modelName, 'Bore Diameter/1', 'Displacement Volume/1');
add_line(modelName, 'Bore Diameter/1', 'Displacement Volume/2');

% Create other necessary blocks for the calculations
% Here, you can follow a similar approach to create blocks for each calculation
% such as clearance volume, total volume, and so on.

% Example for connecting other blocks (not all calculations shown for brevity)
% Connect your blocks as needed using add_line

% Add Scope block to visualize results
add_block('simulink/Commonly Used Blocks/Scope', [modelName, '/Results Scope']);
% Connect results to the Scope
add_line(modelName, 'Displacement Volume/1', 'Results Scope/1');

% Save and open the model
save_system(modelName);
open_system(modelName);
