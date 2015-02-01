#Ruby Style
Style, Refactoring and CodeSmells

##Sandi Mets Rules
###No mothod can be longer than
###No calss can be longer than 100 lines
###Only ever send one instance method form the controller to the view 


##Scopes

    scope :all_active, -> { where(status: "active") }

    class Products < ActiveRecord::Base
      default_scope { where(status: "active") }
    end

    class ProductsController < applicationController
      def index
        @production = Product.all_active
      end
    end
    class ProductsController < applicationController
      def index
        @production = Product.all_active
      end
    end


##class methods

    class Event
      attr_reader :time, name

      def initialize(attrs={})
        @time     = attrs.time
        @name     = attrs.name
        @location = attrs.location
      end

      def self.find(attrs)
        attrs = #DB lookup that ret
        new(attrs)
      end
    end


##Presenters in rails controllers
Creating a single instance veriable in the controller to pass to the view

    class HomeController < ApplicationController
      def show
        @presenter = HomePagePresenter.new(params[:category_id])
      end
    end

    class HomePagePresenter
      attr_reader 
      def initialize(category_id)
       @category_id = category_id
      end

      def categories
        Category.all.sort
      end

      def products
        Products.essentials.sample(3)
      end

      def category_messages
        if category_id == "food"
          "Do some stuff"
        else
          "Do somthing else"
        end
      end
    end







