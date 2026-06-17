classdef DPS1 < matlab.apps.AppBase

    % Properties that correspond to app components
    properties (Access = public)
        UIFigure                    matlab.ui.Figure
        LoadDataButton              matlab.ui.control.Button
        DYNAMICPRICINGSYSTEMLabel   matlab.ui.control.Label
        retail_priceLabel           matlab.ui.control.Label
        Predict_retail_priceButton  matlab.ui.control.Button
        dateDatePicker              matlab.ui.control.DatePicker
        dateDatePickerLabel         matlab.ui.control.Label
        categoryDropDown            matlab.ui.control.DropDown
        categoryDropDownLabel       matlab.ui.control.Label
        varietyDropDown             matlab.ui.control.DropDown
        varietyDropDownLabel        matlab.ui.control.Label
        centreDropDown              matlab.ui.control.DropDown
        centreDropDownLabel         matlab.ui.control.Label
        unitDropDown                matlab.ui.control.DropDown
        unitDropDownLabel           matlab.ui.control.Label
        commodityDropDown           matlab.ui.control.DropDown
        commodityDropDownLabel      matlab.ui.control.Label
        stateDropDown               matlab.ui.control.DropDown
        stateDropDownLabel          matlab.ui.control.Label
    end

    % Callbacks that handle component events
    methods (Access = private)

        % Button pushed function: Predict_retail_priceButton
        function Predict_retail_priceButtonPushed(app, event)
            state = string(app.stateDropDown.Value);
            centre = string(app.centreDropDown.Value);
            commodity = string(app.commodityDropDown.Value);
            variety = string(app.varietyDropDown.Value);
            unit = string(app.unitDropDown.Value);
            category = string(app.categoryDropDown.Value);

            dt = app.dateDatePicker.Value;

            Day = day(dt);
            Month = month(dt);
            Year = year(dt);

            M = load("dps.mat");

            T = table(state, centre, commodity, variety, unit, category, Day, Month, Year, ...
                'VariableNames', {'state','centre','commodity','variety','unit','category','Day','Month','Year'});

            yfit = predict(M.DPSmodel, T);

            app.retail_priceLabel.Text = ['Predicted Retail Price: ₹ ' num2str(yfit)];
        end

        % Button pushed function: LoadDataButton
        function LoadDataButtonPushed(app, event)
            opts = detectImportOptions('DPS(f).csv');
            opts = setvaropts(opts,'date','InputFormat','dd/MM/yyyy');

            data = readtable('DPS(f).csv',opts);

            app.stateDropDown.Items = cellstr(unique(string(data.state)));
            app.centreDropDown.Items = cellstr(unique(string(data.centre)));
            app.commodityDropDown.Items = cellstr(unique(string(data.commodity)));
            app.varietyDropDown.Items = cellstr(unique(string(data.variety)));
            app.unitDropDown.Items = cellstr(unique(string(data.unit)));
            app.categoryDropDown.Items = cellstr(unique(string(data.category)));
        end
    end

    % Component initialization
    methods (Access = private)

        % Create UIFigure and components
        function createComponents(app)

            % Create UIFigure and hide until all components are created
            app.UIFigure = uifigure('Visible', 'off');
            app.UIFigure.Color = [0.0667 0.4431 0.7451];
            app.UIFigure.Position = [100 100 640 480];
            app.UIFigure.Name = 'MATLAB App';

            % Create stateDropDownLabel
            app.stateDropDownLabel = uilabel(app.UIFigure);
            app.stateDropDownLabel.BackgroundColor = [1 1 1];
            app.stateDropDownLabel.HorizontalAlignment = 'right';
            app.stateDropDownLabel.Position = [77 336 31 22];
            app.stateDropDownLabel.Text = 'state';

            % Create stateDropDown
            app.stateDropDown = uidropdown(app.UIFigure);
            app.stateDropDown.BackgroundColor = [1 1 1];
            app.stateDropDown.Position = [123 336 100 22];

            % Create commodityDropDownLabel
            app.commodityDropDownLabel = uilabel(app.UIFigure);
            app.commodityDropDownLabel.BackgroundColor = [1 1 1];
            app.commodityDropDownLabel.HorizontalAlignment = 'right';
            app.commodityDropDownLabel.Position = [43 288 65 22];
            app.commodityDropDownLabel.Text = 'commodity';

            % Create commodityDropDown
            app.commodityDropDown = uidropdown(app.UIFigure);
            app.commodityDropDown.BackgroundColor = [1 1 1];
            app.commodityDropDown.Position = [123 288 100 22];

            % Create unitDropDownLabel
            app.unitDropDownLabel = uilabel(app.UIFigure);
            app.unitDropDownLabel.BackgroundColor = [1 1 1];
            app.unitDropDownLabel.HorizontalAlignment = 'right';
            app.unitDropDownLabel.Position = [83 230 25 22];
            app.unitDropDownLabel.Text = 'unit';

            % Create unitDropDown
            app.unitDropDown = uidropdown(app.UIFigure);
            app.unitDropDown.BackgroundColor = [1 1 1];
            app.unitDropDown.Position = [123 230 100 22];

            % Create centreDropDownLabel
            app.centreDropDownLabel = uilabel(app.UIFigure);
            app.centreDropDownLabel.BackgroundColor = [1 1 1];
            app.centreDropDownLabel.HorizontalAlignment = 'right';
            app.centreDropDownLabel.Position = [401 336 39 22];
            app.centreDropDownLabel.Text = 'centre';

            % Create centreDropDown
            app.centreDropDown = uidropdown(app.UIFigure);
            app.centreDropDown.BackgroundColor = [1 1 1];
            app.centreDropDown.Position = [455 336 100 22];

            % Create varietyDropDownLabel
            app.varietyDropDownLabel = uilabel(app.UIFigure);
            app.varietyDropDownLabel.BackgroundColor = [1 1 1];
            app.varietyDropDownLabel.HorizontalAlignment = 'right';
            app.varietyDropDownLabel.Position = [399 288 41 22];
            app.varietyDropDownLabel.Text = 'variety';

            % Create varietyDropDown
            app.varietyDropDown = uidropdown(app.UIFigure);
            app.varietyDropDown.BackgroundColor = [1 1 1];
            app.varietyDropDown.Position = [455 288 100 22];

            % Create categoryDropDownLabel
            app.categoryDropDownLabel = uilabel(app.UIFigure);
            app.categoryDropDownLabel.BackgroundColor = [1 1 1];
            app.categoryDropDownLabel.HorizontalAlignment = 'right';
            app.categoryDropDownLabel.Position = [388 230 52 22];
            app.categoryDropDownLabel.Text = 'category';

            % Create categoryDropDown
            app.categoryDropDown = uidropdown(app.UIFigure);
            app.categoryDropDown.BackgroundColor = [1 1 1];
            app.categoryDropDown.Position = [455 230 100 22];

            % Create dateDatePickerLabel
            app.dateDatePickerLabel = uilabel(app.UIFigure);
            app.dateDatePickerLabel.BackgroundColor = [1 1 1];
            app.dateDatePickerLabel.HorizontalAlignment = 'right';
            app.dateDatePickerLabel.Position = [233 165 28 22];
            app.dateDatePickerLabel.Text = 'date';

            % Create dateDatePicker
            app.dateDatePicker = uidatepicker(app.UIFigure);
            app.dateDatePicker.Position = [276 165 150 22];

            % Create Predict_retail_priceButton
            app.Predict_retail_priceButton = uibutton(app.UIFigure, 'push');
            app.Predict_retail_priceButton.ButtonPushedFcn = createCallbackFcn(app, @Predict_retail_priceButtonPushed, true);
            app.Predict_retail_priceButton.BackgroundColor = [1 1 1];
            app.Predict_retail_priceButton.Position = [144 124 117 22];
            app.Predict_retail_priceButton.Text = 'Predict_retail_price';

            % Create retail_priceLabel
            app.retail_priceLabel = uilabel(app.UIFigure);
            app.retail_priceLabel.BackgroundColor = [1 0.9569 0.7216];
            app.retail_priceLabel.FontWeight = 'bold';
            app.retail_priceLabel.Position = [219 72 234 22];
            app.retail_priceLabel.Text = 'retail_price';

            % Create DYNAMICPRICINGSYSTEMLabel
            app.DYNAMICPRICINGSYSTEMLabel = uilabel(app.UIFigure);
            app.DYNAMICPRICINGSYSTEMLabel.BackgroundColor = [1 1 1];
            app.DYNAMICPRICINGSYSTEMLabel.FontName = 'Times New Roman';
            app.DYNAMICPRICINGSYSTEMLabel.FontSize = 24;
            app.DYNAMICPRICINGSYSTEMLabel.FontWeight = 'bold';
            app.DYNAMICPRICINGSYSTEMLabel.Position = [144 400 383 31];
            app.DYNAMICPRICINGSYSTEMLabel.Text = '🏷️DYNAMIC PRICING SYSTEM ';

            % Create LoadDataButton
            app.LoadDataButton = uibutton(app.UIFigure, 'push');
            app.LoadDataButton.ButtonPushedFcn = createCallbackFcn(app, @LoadDataButtonPushed, true);
            app.LoadDataButton.BackgroundColor = [1 1 1];
            app.LoadDataButton.Position = [400 124 103 22];
            app.LoadDataButton.Text = 'LoadDataButton';

            % Show the figure after all components are created
            app.UIFigure.Visible = 'on';
        end
    end

    % App creation and deletion
    methods (Access = public)

        % Construct app
        function app = DPS1

            % Create UIFigure and components
            createComponents(app)

            % Register the app with App Designer
            registerApp(app, app.UIFigure)

            if nargout == 0
                clear app
            end
        end

        % Code that executes before app deletion
        function delete(app)

            % Delete UIFigure when app is deleted
            delete(app.UIFigure)
        end
    end
end